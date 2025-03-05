# Sessions Funnel

## Visão Geral
A tabela `sessions_funnel.sqlx` combina dados de sessões com informações de compras, criando uma visão unificada do funil de conversão com enriquecimento de dados UTM.

## Configurações
- **Tipo**: Table
- **Schema**: dataform_join
- **Particionamento**: Por data do evento (event_date)

## Estrutura e Funcionamento

### 1. Etapas de Processamento
1. Agrega dados de compras por usuário e sessão
2. Combina com dados de sessões
3. Enriquece UTMs com descrições das tabelas de códigos

### 2. Classificação de Páginas
- Categoriza URLs em diferentes grupos de produtos/campanhas
- Identifica páginas específicas de produtos
- Classifica tipos de conteúdo

## Uso
- Análise de funil de conversão
- Avaliação de performance por produto
- Análise de comportamento por origem de tráfego

## Dependências
- orders
- sessions
- utm_codes_medium
- utm_codes_source
- utm_codes_campaign

## Campos Adicionados
- page_location_group: Classificação da página em grupos de produtos
- Descrições normalizadas de medium, source e campaign 