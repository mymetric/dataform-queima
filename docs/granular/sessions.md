# Sessions

## Visão Geral
A tabela `sessions.sqlx` é uma tabela incremental que armazena informações detalhadas sobre as sessões dos usuários no site, com foco especial nas fontes de tráfego e parâmetros de campanha.

## Configurações
- **Tipo**: Incremental
- **Schema**: dataform_granular
- **Frequência**: Atualização diária
- **Particionamento**: Por data do evento (event_date)

## Estrutura e Funcionamento

### 1. Pré-operações
- Define uma data máxima para processamento incremental
- Para primeira execução, começa em 2019-01-01
- Em execuções subsequentes, usa a última data processada

### 2. Principais Etapas de Processamento

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

#### Tratamento de Campos
- **Source**: 
  - Se tem gclid → "google"
  - Se tem source → valor do source
  - Caso contrário → "direct"

- **Medium**:
  - Se tem gclid → "cpc"
  - Se tem medium → valor do medium
  - Caso contrário → "direct"

- **Outros Campos**:
  - Campaign: valor ou "(not set)"
  - Content: valor ou "(not set)"
  - Term: valor ou "(not set)"
  - GCLID: valor ou "(not set)"

#### Tratamento da URL
- Separa a URL em duas partes:
  1. page_location: URL base sem parâmetros
  2. page_params: Parâmetros da URL (se existirem)

## Uso
Esta tabela é útil para:
- Análise de fontes de tráfego
- Atribuição de campanhas
- Análise de comportamento de sessões
- Rastreamento de campanhas pagas (Google Ads, Facebook)

## Limitações
- Processa apenas dados da plataforma WEB
- Exclui eventos de compra ("purchase")
- Não inclui dados intraday

## Campos de Saída

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