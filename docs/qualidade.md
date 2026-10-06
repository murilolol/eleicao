# Qualidade da versão documentada

Referência: 05/10/2026, horário de Brasília. Este registro descreve as verificações da implementação privada; não distribui os testes nem oferece uma certificação de ausência universal de falhas.

## Verificações realizadas

| Verificação | Resultado registrado |
| --- | --- |
| Testes automatizados | 131 testes passaram na atualização mais recente |
| Checagem de UI | 83 arquivos examinados, sem referências indefinidas |
| Build de produção | Concluído; PWA gerada, APIs fora do cache do aplicativo |
| Navegação municipal | Prefeitos e vereadores, mesma cidade, anos históricos e retorno à apuração de 2026 |
| Turnos | São Paulo no segundo turno; visão final sem somar dois turnos ou duplicar cidades |
| Chapas | Prefeito e vice de partidos diferentes, com votação conjunta explícita |
| Listas | Todas as candidaturas, filtros e paginação além da primeira página |
| Patrimônio | Identidade oficial, centavos, categorias, ausência distinta de zero e vínculo válido no segundo turno |
| Responsividade | Chrome em 320 px, 390 px e desktop, com capturas adicionais em 4K |
| Mapa e rodapé | Zoom pela roda do mouse; mapa no fluxo da página e rodapé estático |
| Documentação e imagens | Links locais, dimensões e checksums verificados |

## Contratos protegidos

Parsers e APIs são verificados com pequenas fixtures de arquivos oficiais. As validações incluem schemas observados, códigos territoriais, somas, percentuais, cargo, turno, situação histórica, ausência de sequencial e campos privados excluídos.

Falha de refresh ou de publicação final preserva a versão anterior. Mudanças no adaptador geram snapshots isolados mesmo quando o ZIP oficial não mudou. A identidade inclui eleição e território; não é deduzida pelo nome da pessoa. Patrimônio e votação mantêm períodos e tipos de referência separados.

## Conferência visual

A revisão considerou hierarquia, movimento reduzido, filtros, foco de diálogos, fotos disponíveis, largura da página e acesso a detalhes. A experiência municipal reutiliza a identidade do restante do produto. No celular, a tabela patrimonial pode rolar internamente sem alargar a página.

As capturas não representam todos os dispositivos, recortes ou combinações de filtros. Capacidade de produção, novos agendamentos e cargas ainda pendentes são descritos na [cobertura](cobertura.md), sem afirmar que já foram concluídos.

## Publicação e compartilhamento

A atualização de frontend e backend foi publicada sem transportar bancos, caches ou bases analíticas maiores. A apuração ao vivo permanece independente; o aviso de sincronização continua visível até a carga dessas bases.

- Build de produção concluído na VPS; 12 testes selecionados passaram no ambiente Node.js 20.
- Verificação HTTPS de títulos, descrições e links canônicos por localidade, incluindo ano histórico.
- Arte Open Graph entregue como JPEG de 1.200 × 630 px; metadados de imagem e Twitter Cards presentes no HTML.
- `robots.txt` e sitemap conferidos; 144 URLs nesta etapa, com páginas analíticas pendentes marcadas como `noindex`.
- Novo texto de apoio e privacidade presente no aplicativo publicado; código Pix preservado.
- Coletor ao vivo saudável na verificação e aviso de sincronização mantido.

Essas verificações usam respostas HTTP, HTML e contratos automatizados. A galeria e a conferência visual acima pertencem à versão local capturada anteriormente; não foi feita nova captura de navegador para esta atualização.

## Como conferir as capturas

O [manifesto](../media/manifest.json) registra arquivo, formato, dimensões, tamanho, captura em BRT e SHA-256. Desktop 4K significa largura nativa de 3.840 px. As páginas completas podem ultrapassar 2.160 px de altura. Imagens móveis e painéis têm enquadramento identificado na [galeria](galeria.md).

## Armazenamento compacto

127 testes passaram após a evolução dos leitores. A manutenção verifica SHA-256 dos documentos e integridade dos stores. As 19 respostas reais usadas como referência permanecem idênticas antes e depois da limpeza e compactação. A [documentação de armazenamento](armazenamento.md) informa as medidas, a preservação de dados e os limites da estimativa para produção.
