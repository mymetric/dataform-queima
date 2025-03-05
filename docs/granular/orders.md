# Orders

## Visão Geral
A tabela `orders.sqlx` é uma tabela incremental que armazena informações detalhadas sobre os pedidos realizados, incluindo dados do cliente, informações do pedido e dados de rastreamento.

## Configurações
- **Tipo**: Incremental
- **Schema**: dataform_granular
- **Particionamento**: Por data do pedido (date)

## Estrutura e Funcionamento

### 1. Pré-operações
- Define uma data máxima para processamento incremental
- Para primeira execução, começa em 2023-03-12
- Em execuções subsequentes, usa a última data processada

### 2. Principais Etapas de Processamento

#### a) Extração de JSON (json)
Extrai e estrutura dados do JSON fonte, incluindo:
- Informações do pedido (order_id, status, revenue)
- Dados do cliente (email, telefone)
- Cookies e rastreamento (user_pseudo_id, ga_session_id, fbp, fbc)
- Informações técnicas (IP, page_location, user_agent)
- Dados do produto (id, nome, quantidade, preço)

#### b) Deduplicação (dedup)
- Remove duplicatas mantendo apenas o registro mais recente
- Considera a combinação de order_id e status como chave de deduplicação
- Ordena por datetime em ordem decrescente

### 3. Transformações
- Tratamento de campos nulos com COALESCE
- Extração segura de valores do JSON
- Conversão de timestamp para datetime na timezone America/Sao_Paulo

## Uso
Esta tabela é útil para:
- Análise de vendas
- Acompanhamento de pedidos
- Análise de comportamento de compra
- Integração com dados de sessão para atribuição

## Limitações
- Exclui dados do dia atual
- Processa apenas pedidos posteriores a 2023-03-12
- Depende da estrutura do JSON de entrada

## Campos de Saída

| Campo | Tipo | Descrição |
|-------|------|-----------|
| date | DATE | Data do pedido |
| datetime | DATETIME | Data e hora do pedido (America/Sao_Paulo) |
| order_id | STRING | ID do pedido |
| status | STRING | Status do pedido ou motivo de cancelamento |
| revenue | STRING | Valor total do pedido |
| email | STRING | Email do usuário |
| phone | STRING | Telefone do usuário |
| user_pseudo_id | STRING | ID pseudônimo do usuário |
| ga_session_id | INTEGER | ID da sessão do Google Analytics |
| fbp | STRING | Facebook Browser ID |
| fbc | STRING | Facebook Click ID |
| ip | STRING | IP do usuário |
| page_location | STRING | URL da página |
| user_agent | STRING | User agent do navegador |
| product_id | STRING | ID do produto |
| product_name | STRING | Nome do produto |
| product_quantity | STRING | Quantidade do produto |
| product_price | STRING | Preço unitário do produto | 