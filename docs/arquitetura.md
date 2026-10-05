# Arquitetura da plataforma

O Acompanhar Eleição separa a apuração em tempo real das estatísticas cadastrais, históricas, mensais e censitárias. Este documento apresenta os contratos conceituais; não distribui a implementação ou instruções de acesso à infraestrutura.

## Visão geral

```mermaid
graph TD
    TSE[TSE: boletins oficiais] --> FILA[Coletor compartilhado]
    FILA --> CACHE[Último boletim válido e histórico de publicações]
    CACHE --> API[API de leitura]
    CACHE --> WS[WebSocket e presença]
    CATALOG[TSE: catálogo CKAN] --> IMPORT[Jobs de importação por fonte]
    GOVERNO[IBGE e MDS: bases agregadas] --> IMPORT
    IMPORT --> RAW[Original agregado ou referência oficial versionada]
    RAW --> STAGING[Conversão em fluxo e campos públicos auditados]
    STAGING --> VALIDACAO[Schema, identidade, somas e geografia]
    VALIDACAO --> AGREGA[Perfis e agregados locais]
    AGREGA --> API
    API --> SITE[Interface responsiva]
    WS --> SITE
```

A visita de um usuário consulta o backend do projeto. Não dispara uma coleta independente de resultados no TSE. Fotografias têm cache próprio; dados analíticos são servidos de versões importadas e consultas financeiras indexadas.

## Fluxo de atualização — UML de sequência

```mermaid
sequenceDiagram
    participant T as Fonte oficial
    participant C as Coletor compartilhado
    participant A as Cache e API
    participant U as Interface do visitante
    C->>T: Consultar arquivo elegível da fila
    T-->>C: Boletim e horário oficial
    C->>C: Validar identidade e campos
    C->>A: Preservar último resultado válido
    A-->>U: Publicar atualização por WebSocket
    U->>U: Atualizar recorte, contagens e mapa
    Note over C,U: Todos os visitantes compartilham a coleta
    alt Fonte falha ou limita consultas
        T-->>C: Erro ou indisponibilidade
        C->>C: Recuo e reagendamento
        A-->>U: Cache válido com referência temporal
    end
```

O coletor consulta cada arquivo com intervalo mínimo de 13 segundos, em fila compartilhada e com limite global mais conservador. HTTP 429 suspende a coleta conforme o recuo e o `Retry-After`. Esses limites são uma estratégia do projeto, não uma afirmação de garantia de serviço da fonte.

## Domínio dos dados — UML conceitual

```mermaid
classDiagram
    class Localidade {
        nivelGeografico
        codigoTSE
        codigoIBGE
        uf
    }
    class Eleicao {
        ano
        identificadorOficial
        turno
        cargo
    }
    class Candidatura {
        sequencialOficial
        numeroUrna
        partido
        situacaoCadastral
    }
    class Resultado {
        votos
        votosValidos
        secoesTotalizadas
        horarioPublicacao
    }
    class PerfilAgregado {
        referenciaCadastro
        totalEleitores
        distribuicoesOficiais
    }
    class DeclaracaoFinanceira {
        dataEntrega
        receitasFinanceiras
        recursosEstimaveis
        despesasContratadas
        pagamentos
    }
    class Proveniencia {
        fonte
        recurso
        periodo
        granularidade
        checksum
        importacao
    }
    Eleicao "1" --> "muitas" Candidatura : identifica
    Localidade "1" --> "muitos" Resultado : recorte
    Candidatura "1" --> "muitos" Resultado : recebe votos
    Localidade "1" --> "muitos" PerfilAgregado : referencia
    Candidatura "1" --> "muitas" DeclaracaoFinanceira : declara
    Resultado --> Proveniencia : documenta
    PerfilAgregado --> Proveniencia : documenta
    DeclaracaoFinanceira --> Proveniencia : documenta
```

O diagrama descreve o domínio, não afirma que todo conceito é uma tabela SQL única. A persistência atual combina JSONs pré-calculados, SQLite indexado e referências versionadas. A identidade de candidatura conserva ano, eleição, turno, UF e sequencial oficial; números de urna ou nomes não bastam para relacionar pessoas entre eleições.

## Navegação — UML de estados

```mermaid
stateDiagram-v2
    [*] --> Resultado
    Resultado --> Mapa: explorar território
    Mapa --> Resultado: consultar votação
    Resultado --> Eleitorado: abrir perfil cadastral
    Eleitorado --> Contexto: consultar IBGE e programas sociais
    Contexto --> Historico: comparar eleição importada
    Historico --> Resultado: voltar à apuração
    Resultado --> Candidatura: abrir retrato
    Candidatura --> Resultado: fechar perfil
    note right of Eleitorado
        UF e município são preservados
        nos temas da localidade.
    end note
```

## Falhas independentes

Uma fonte analítica indisponível não elimina o boletim LIVE nem as outras fontes do perfil. Uma publicação inválida não substitui silenciosamente o último dado válido. As telas informam dados indisponíveis, divergências, referências temporais e cobertura parcial.

## Histórico municipal

Prefeitos e vereadores utilizam um pipeline separado da apuração de 2026. ZIPs oficiais passam por leitura em streaming, validação do schema e staging SQLite. Snapshots pré-calculados por eleição, município e zona são publicados por manifest versionado. A versão do adaptador participa do checksum, isolando reprocessamentos de um mesmo arquivo. A API serve o cache local; a visita de uma pessoa não dispara nova consulta de resultados ao TSE.

```mermaid
graph TD
    A[Catálogo oficial do TSE] --> B[Arquivos de resultados e candidaturas]
    B --> C[Staging validado e campos públicos]
    C --> D[Vereadores por ano e zona]
    C --> E[Prefeitos por ano e turno]
    E --> F[Último turno disponível de cada cidade]
    D --> G[Snapshots versionados e API local]
    F --> G
    G --> H[Resultado, mapa, participação e histórico]
```
