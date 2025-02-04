# Purchases Intraday

## Visão Geral
A tabela `purchases_intraday.sqlx` é uma tabela que combina dados de compras do dia atual (intraday) com o histórico de compras, fornecendo uma visão completa e atualizada das transações.

## Configurações
- **Tipo**: Table
- **Schema**: dataform_granular
- **Tags**: daily
- **Particionamento**: Por data do evento (event_date)

## Estrutura e Funcionamento

### 1. Pré-operações
- Obtém a data máxima da tabela de purchases

### 2. Processamento
- Captura eventos de compra intraday
- União com dados históricos da tabela purchases
- Mantém a mesma estrutura de dados da tabela purchases

## Uso
- Análise em tempo real de vendas
- Dashboards operacionais
- Monitoramento de transações

## Campos de Saída
(Mesma estrutura da tabela purchases) 