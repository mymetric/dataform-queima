# Sessions Clustered

## Visão Geral
A view `sessions_clustered.sqlx` enriquece os dados de sessões com classificações de páginas e normalização de UTMs, permitindo análises agrupadas por produtos e campanhas.

## Configurações
- **Tipo**: View
- **Schema**: dataform_join
- **Tags**: daily

## Estrutura e Funcionamento

### 1. Etapas de Processamento

#### a) Classificação de Páginas (prep)
- Categoriza URLs em grupos de produtos usando REGEXP_CONTAINS
- Principais categorias:
  - Queima Diária
  - Treinos Power
  - Mamãe Sarada
  - Pump 10
  - Barriga Negativa
  - Adeus Diástase
  - Xô Flacidez
  - E outros produtos específicos

#### b) Normalização de UTMs (prep2)
- Enriquece dados com descrições normalizadas de:
  - Medium
  - Source
  - Campaign
- Usa tabelas de referência para padronização

#### c) Deduplicação (prep3/prep4)
- Remove duplicatas mantendo primeira sessão do dia
- Extrai informações adicionais:
  - Dia da semana
  - Hora
  - Parâmetros de fonte (src)

### 2. Campos Calculados
- src_flag: Identifica sessões com gclid nos parâmetros
- page_location_group: Classificação da página em grupos de produtos

## Uso
- Análise de tráfego por produto
- Segmentação de campanhas
- Análise de comportamento por origem

## Dependências
- sessions_intraday
- utm_codes_medium
- utm_codes_source
- utm_codes_campaign

## Campos Adicionados

| Campo | Tipo | Descrição |
|-------|------|-----------|
| page_location_group | STRING | Classificação da página (produto/campanha) |
| day_of_week | INTEGER | Dia da semana (1-7) |
| hour | INTEGER | Hora do dia (0-23) |
| src | STRING | Parâmetro de fonte da URL |
| src_flag | INTEGER | Flag para sessões com gclid (0/1) |
| medium | STRING | Meio normalizado |
| source | STRING | Fonte normalizada |
| campaign | STRING | Campanha normalizada | 