# Sessions Intraday

## Visão Geral
A tabela `sessions_intraday.sqlx` combina dados de sessões do dia atual (intraday) com o histórico de sessões, mantendo a mesma estrutura e lógica de processamento da tabela sessions.

## Configurações
- **Tipo**: Table
- **Schema**: dataform_granular
- **Tags**: daily
- **Particionamento**: Por data do evento (event_date)

## Estrutura e Funcionamento

### 1. Pré-operações
- Obtém a data máxima da tabela sessions
- Usa esta data como referência para processamento incremental

### 2. Etapas de Processamento

#### a) Extração de Parâmetros (traffic_sources)
Coleta dados básicos da sessão incluindo:
- Data e timestamp do evento
- ID do usuário
- ID da sessão
- Parâmetros UTM (source, medium, campaign, content, term)
- Parâmetros de rastreamento (gclid, fbp, fbc)
- Localização da página
- Categoria do dispositivo

#### b) Processamento de Tráfego
Divide o processamento em duas categorias:

1. **Tráfego Não-Direto** (non_direct_traffic)
   - Inclui sessões com fonte (source) definida
   - Mantém apenas o primeiro evento de cada sessão

2. **Tráfego Direto** (direct_traffic)
   - Inclui sessões sem fonte definida
   - Processa separadamente do tráfego não-direto
   - Mantém apenas o primeiro evento de cada sessão

### 3. Transformações Finais
- Tratamento de campos source/medium:
  - Se tem gclid → source="google", medium="cpc"
  - Caso contrário → usa valores originais ou "direct"
- Separação da URL em base e parâmetros
- União com dados históricos dos últimos 400 dias

## Uso
- Análise em tempo real de sessões
- Monitoramento de tráfego atual
- Dashboards operacionais

## Limitações
- Processa apenas dados da plataforma WEB
- Exclui eventos de compra ("purchase")
- Inclui apenas dados intraday e histórico recente (400 dias)

## Dependências
- sessions (tabela base)

## Campos de Saída
(Mesma estrutura da tabela sessions)

| Campo | Tipo | Descrição |
|-------|------|-----------|
| event_date | DATE | Data do evento |
| event_timestamp | TIMESTAMP | Timestamp do evento |
| user_pseudo_id | STRING | ID pseudônimo do usuário |
| ga_session_id | INTEGER | ID da sessão do Google Analytics |
| category | STRING | Categoria do dispositivo |
| source | STRING | Fonte do tráfego |
| medium | STRING | Meio do tráfego |
| campaign | STRING | Nome da campanha |
| content | STRING | Conteúdo da campanha |
| term | STRING | Termo de busca |
| gclid | STRING | ID do Google Click |
| page_location | STRING | URL base da página |
| page_params | STRING | Parâmetros da URL | 