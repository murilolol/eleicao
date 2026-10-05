# Fontes e metodologia

Projeto independente. Os dados não constituem declaração, serviço ou chancela institucional do TSE, IBGE ou MDS.

## Referências separadas

| Tipo | O que representa |
|---|---|
| LIVE | Boletins de apuração, com publicação oficial e consulta pelo coletor |
| SNAPSHOT | Cadastro eleitoral, candidaturas, patrimônio e declarações em uma referência publicada |
| MONTHLY | Indicadores agregados de programas sociais em uma competência mensal |
| CENSUS | Dados censitários em sua referência e revisão |
| ANNUAL | Séries econômicas anuais, com unidade e metodologia próprias |

Conexão ativa não significa que o TSE publicou um novo boletim. A interface distingue horário de publicação, horário de consulta e disponibilidade da conexão. Totalizar todas as seções não impede revisões posteriores da fonte.

## Fontes primárias

- [Portal de Dados Abertos do TSE](https://dadosabertos.tse.jus.br/): descoberta de recursos CKAN e arquivos/layouts oficiais.
- [Resultados oficiais do TSE](https://resultados.tse.jus.br/): boletins eleitorais e divulgação pública.
- [IBGE — Censo Demográfico](https://www.ibge.gov.br/estatisticas/sociais/populacao/22827-censo-demografico-2022.html): população e referência censitária.
- [IBGE — PIB dos Municípios](https://www.ibge.gov.br/estatisticas/economicas/contas-nacionais/9088-produto-interno-bruto-dos-municipios.html): série econômica municipal.
- [MDS — VIS DATA](https://aplicacoes.cidadania.gov.br/vis/data3/data-explorer.php): indicadores sociais agregados.

Cada recurso importado conserva a fonte, a URL, o período, o checksum, o schema observado e o momento de importação. Atualização do catálogo não substitui a data de geração do arquivo.

## Geografia e denominadores

Municípios são relacionados por códigos oficiais e UF, com equivalências TSE/IBGE validadas. Não há JOIN somente por nome. Exterior conserva a geografia eleitoral própria, sem receber um código IBGE municipal fictício.

Um indicador municipal não é distribuído artificialmente por zona ou seção. Programas sociais e indicadores econômicos permanecem municipais, salvo agregações territoriais válidas. BPC desta carga utiliza município pagador, que não é apresentado como domicílio de cada beneficiário.

Percentuais demográficos usam o total do cadastro, incluindo “não informado”. Ausência de registro não prova ausência da característica. As características autodeclaradas são identificadas conforme a fonte.

## Voto secreto e associações territoriais

Análises demográficas são realizadas a partir de dados agregados territoriais. Elas não identificam nem permitem determinar como indivíduos ou grupos específicos votaram.

Uma associação entre composição demográfica de seções ou municípios e votação não descreve o voto individual. Correlação não prova causalidade. A plataforma não afirma que determinado grupo “preferiu” uma candidatura a partir de cruzamentos agregados.

## Campanha e patrimônio

Valores declarados pelo próprio candidato à Justiça Eleitoral. Patrimônio declarado não representa renda. A entrega de contas mais recente disponível não equivale a aprovação final.

Receitas financeiras, recursos estimáveis, contratação e pagamentos permanecem separados. Doadores originários detalham repasses e não são acrescentados novamente à receita. Não se calcula saldo bancário presumido. Valores por voto usam a abrangência completa da candidatura e exibem referências contábeis/eleitorais distintas.

Nomes públicos de doadores e fornecedores aparecem somente no detalhamento autorizado dos registros. CPF, NIS, título de eleitor, documentos e contatos pessoais não são publicados no módulo analítico. Não há consulta individual de beneficiários nem tentativa de identificação do voto.

## Estimativas e histórico

A estimativa de encerramento usa o ritmo recente e seções restantes. Mostra uma faixa de cenário com premissas; não é intervalo estatístico garantido nem previsão de vencedor. Amostras antigas, paradas ou regressivas não geram uma previsão artificial.

Comparações conservam cargo, turno e referência. Históricos cadastrais de candidaturas não substituem resultados de votação de outros anos. Mudanças de metodologia são indicadas; ausências permanecem indisponíveis.

## Eleições municipais

Prefeitos e vereadores são apresentados apenas nos anos de eleições ordinárias de 1996 a 2024. Eleição de prefeito é a situação oficial do TSE; liderança de votos não substitui esse status. O vice recebe os votos da chapa e não tem votação individual. A visão final escolhe o último turno publicado para cada município, sem somar turnos nem contar uma cidade duas vezes. Nas UFs, distribuição nominal por partido não é uma disputa estadual entre prefeitos.

As bases antigas podem distinguir votos nominais informados de válidos de forma diferente. Denominadores, turno e ano ficam explícitos. O total válido originalmente publicado é preservado ao lado da soma calculada de nominais + legenda. A malha atual ajuda na navegação, sem redistribuir resultados entre territórios históricos. O TSE informa que 1996 é incompleto. Retratos e biografias só usam a referência do próprio ano; lacunas permanecem visíveis.
