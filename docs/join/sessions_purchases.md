# Sessions Purchases

## Visão Geral
A tabela `sessions_purchases.sqlx` combina dados de sessões com informações detalhadas de compras, fornecendo uma visão completa das conversões por sessão e tipo de produto.

## Configurações
- **Tipo**: Table
- **Schema**: dataform_join
- **Tags**: daily
- **Particionamento**: Por data do evento (event_date)

## Estrutura e Funcionamento

### 1. Filtragem de Sessões
Remove sessões de ambientes não produtivos:
- Ambientes de desenvolvimento
- Páginas de teste
- URLs internas
- Ambientes de homologação

### 2. Métricas de Compra
Calcula métricas separadas para diferentes tipos de produtos:

#### Produtos Principais
- Número de transações
- Receita total
- Exclui produtos específicos (IDs: 323, 190, 193, etc.)

#### Produtos Específicos
- **Nutricional**
  - Transações e receita (ID: 323)
- **Mamãe Cream**
  - Transações e receita (IDs: 10001, 10002, 10003)
- **Coaching**
  - Transações e receita (IDs: 196, 369)
- **Coaching B**
  - Transações e receita (ID: 371)
- **Downsell Nutricional**
  - Transações e receita (ID: 370)
- **Nutricional Mamãe Sarada**
  - Transações e receita (ID: 362)
- **Downsell Nutricional Mamãe Sarada**
  - Transações e receita (ID: 373)
- **Coaching Mamãe Sarada**
  - Transações e receita (ID: 372)
- **Upsell Mamãe Cream MS**
  - Transações e receita (ID: 368)

## Uso
- Análise de conversão por sessão
- Performance de produtos específicos
- Análise de upsell/downsell
- Avaliação de receita por tipo de produto

## Dependências
- sessions_clustered
- purchases_last_non_direct

## Campos de Saída

| Campo | Tipo | Descrição |
|-------|------|-----------|
| transactions | INTEGER | Número de transações principais |
| revenue | FLOAT | Receita total principal |
| transactions_nutricional | INTEGER | Transações do produto nutricional |
| revenue_nutricional | FLOAT | Receita do produto nutricional |
| transactions_mamae | INTEGER | Transações Mamãe Cream |
| revenue_mamae | FLOAT | Receita Mamãe Cream |
| transactions_coaching | INTEGER | Transações de coaching |
| revenue_coaching | FLOAT | Receita de coaching |
| transactions_coaching_b | INTEGER | Transações de coaching B |
| revenue_coaching_b | FLOAT | Receita de coaching B |
| transactions_downsell_nutricional | INTEGER | Transações de downsell nutricional |
| revenue_downsell_nutricional | FLOAT | Receita de downsell nutricional |
| transactions_nutricional_mamae_sarada | INTEGER | Transações nutricional Mamãe Sarada |
| revenue_nutricional_mamae_sarada | FLOAT | Receita nutricional Mamãe Sarada |
| transactions_downsell_nutricional_mamae_sarada | INTEGER | Transações downsell Mamãe Sarada |
| revenue_downsell_nutricional_mamae_sarada | FLOAT | Receita downsell Mamãe Sarada |
| transactions_coaching_mamae_sarada | INTEGER | Transações coaching Mamãe Sarada |
| revenue_coaching_mamae_sarada | FLOAT | Receita coaching Mamãe Sarada |
| transactions_upsell_mamae_cream_ms | INTEGER | Transações upsell Mamãe Cream |
| revenue_upsell_mamae_cream_ms | FLOAT | Receita upsell Mamãe Cream | 