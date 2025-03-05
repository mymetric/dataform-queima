# Purchases

## Visão Geral
A tabela `purchases.sqlx` é uma tabela incremental que captura eventos de compra do Google Analytics, armazenando informações detalhadas sobre as transações e seus itens.

## Configurações
- **Tipo**: Incremental
- **Schema**: dataform_granular
- **Tags**: daily
- **Particionamento**: Por data do evento (event_date)

## Estrutura e Funcionamento

### 1. Pré-operações
- Define uma data máxima para processamento incremental
- Para primeira execução, começa em 2019-01-01
- Em execuções subsequentes, usa a última data processada

### 2. Dados Capturados
- Data e timestamp do evento
- ID pseudônimo do usuário
- ID da sessão do GA
- Dados do ecommerce (objeto completo)
- Itens da compra (array de produtos)

## Limitações
- Exclui dados intraday
- Processa apenas eventos do tipo "purchase"

## Campos de Saída

| Campo | Tipo | Descrição |
|-------|------|-----------|
| event_date | DATE | Data do evento |
| event_timestamp | TIMESTAMP | Timestamp do evento |
| user_pseudo_id | STRING | ID pseudônimo do usuário |
| ga_session_id | INTEGER | ID da sessão do Google Analytics |
| ecommerce | RECORD | Objeto com dados da transação |
| items | ARRAY | Array com itens da compra | 