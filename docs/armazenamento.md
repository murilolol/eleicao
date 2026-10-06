# Armazenamento e execução compacta

Referência: 05/10/2026 BRT. Manutenção local documentada. O espaço da VPS foi conferido por SSH; esta manutenção não implantou os dados compactados em produção.

| Medida | Resultado |
| --- | ---: |
| Dados e fotos antes da manutenção | 40.7 GiB |
| Dados e fotos após a manutenção | 10.5 GiB |
| Espaço liberado | aproximadamente 32.5 GB |
| Dados ativos e fotos para consultas | aproximadamente 8.3 GB |
| Documentos compactados sem perda | 684.079 |
| Registros financeiros preservados | 2.003.360 |

GB e GiB são unidades diferentes. Os valores acima medem a ocupação local; outro filesystem pode alocar espaço de forma diferente. A estimativa de execução não inclui aplicação, dependências, sistema operacional nem margem de atualização e backup.

## O que foi feito

Intermediários de importação e versões não referenciadas pelos manifests atuais foram removidos. Arquivos rastreados pelo Git, RAW, base canônica e retratos oficiais permaneceram preservados.

Snapshots completos foram reunidos em stores SQLite, com payloads gzip e SHA-256. O leitor mantém as URLs e respostas da API, aceita JSON recém-publicado e tem fallback por Python. Não houve descarte de campos.

O banco financeiro continua indexado em SQLite. Payloads foram comprimidos sem alterar filtros, centavos, categorias, IDs de linhas ou valores. Contagens, totais e o hash de todos os registros foram conferidos antes da substituição atômica.

## Verificação

127 testes passaram, além de lint e build. As 19 consultas reais de referência retornam dados idênticos antes e depois da manutenção. Todos os documentos compactados passam pela verificação de integridade, além das amostras de API.

O conjunto de execução usa somente versões ativas e fotos necessárias. Downloads, staging e originais de auditoria não precisam acompanhar cada instalação de consulta. Se o servidor também importar novas bases, é necessário considerar a base canônica e espaço temporário. Capacidade de VPS não é deduzida apenas do tamanho final dos arquivos.

Esta vitrine documenta a estratégia e seus resultados. Ela não contém os stores, bancos, snapshots ou código da aplicação.
