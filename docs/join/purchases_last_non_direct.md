# Purchases Last Non Direct

## Visão Geral
A view `purchases_last_non_direct.sqlx` relaciona compras com suas sessões de origem, identificando a última sessão não-direta antes da compra para análise de atribuição.

## Configurações
- **Tipo**: View
- **Schema**: dataform_join
- **Tags**: daily

## Estrutura e Funcionamento

### 1. Etapas de Processamento
1. Separa sessões em diretas e não-diretas
2. Para cada compra:
   - Identifica a última sessão não-direta anterior
   - Se não houver sessão não-direta, busca a sessão direta correspondente

### 2. Lógica de Atribuição
- Para tráfego não-direto: última sessão não-direta antes da compra
- Para tráfego direto: sessão direta da compra

## Uso
- Análise de atribuição de vendas
- Avaliação de efetividade de campanhas
- Entendimento do caminho de conversão

## Dependências
- purchases_intraday
- sessions_clustered 