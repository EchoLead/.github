<p align="center">
  <img src="./docs/echolead-banner.png" alt="EchoLead banner" width="720" />
</p>

# EchoLead

**EchoLead** é uma plataforma **event-driven, multi-tenant e analytics-first** para **captura, enriquecimento e ativação de leads** — conectando **Landing Pages + Chat (Chatwoot) + Console + Integrações (CRM)** em um fluxo único, auditável e preparado para escala.

O foco é resolver o “buraco” entre **marketing → conversa → conversão → CRM**, garantindo que **cada lead chegue com contexto completo**, com **rastreabilidade ponta-a-ponta** e dados consistentes para operação e análise.

---

## Por que a EchoLead existe

Times de marketing e vendas costumam sofrer com:

- **Perda de UTMs / gclid / fbclid** entre páginas, redirecionamentos e início de chat.
- **Conversas sem contexto** (origem, campanha, landing, referrer, dispositivo), gerando atendimento “cego”.
- **Dado fragmentado**: GA4/Pixel dizem uma coisa, CRM outra, chat outra.
- **Integrações frágeis**: duplicidade de leads, sync que falha, falta de idempotência.
- **Dificuldade de auditoria**: “de onde veio esse lead?”, “qual campanha gerou?”, “por que foi roteado pra X?”.
- **Visão operacional ruim**: sem funil real, sem health checks e sem replay.

A EchoLead nasce para **centralizar a inteligência do lead**: captura, persiste, enriquece, aplica regras e sincroniza com os destinos (Chat, CRM, Data Warehouse), mantendo **histórico imutável** para auditoria e reprocessamento.

---

## O que a EchoLead soluciona (na prática)

### 1) Contexto completo do lead (desde o primeiro clique)
- Persistência de **tracking_id** e parâmetros (**UTM, gclid, fbclid, referrer, page_url**).
- Continuidade de contexto entre **landing → navegação → chat → conversão**.
- Metadados prontos para automações (roteamento, scoring, segmentação).

### 2) Chat com inteligência (Chatwoot)
- Widget de chat recebe o lead já identificado e com contexto.
- Possibilita:
  - Inbox por tenant / cliente
  - labels/tags automáticas
  - roteamento baseado em regras
  - histórico consistente (sem “lead perdido”)

### 3) Data pipeline robusto e auditável
- Arquitetura **event-driven** com **fila/mensageria**.
- Armazenamento de **eventos brutos e enriquecidos**.
- **Replay** e **auditoria** por histórico imutável (reprocessar regras sem perder o original).

### 4) Console operacional (control plane)
- KPIs e funil por tenant: volume, origem, campanhas, conversão, SLA, status de integrações.
- Diagnóstico: health, filas, erros, eventos “presos”.
- Ações: replay de eventos, re-sync, inspeção de lead/conversa.

### 5) Sincronização confiável com CRM
- Sync **idempotente** (evita duplicidade).
- Propaga **origem/campanha/conversa** para o CRM.
- Tratamento de falhas com retry e rastreabilidade.

---

## Para quem é

- **Agências** e operações multi-cliente que precisam padronizar tracking e entrega de leads.
- **SaaS / produtos** com múltiplos tenants, cada um com suas landings e canais.
- Times de **growth + SDR/closer** que precisam de contexto no atendimento e dados confiáveis no CRM.
- Operações que querem um **“single source of truth”** do lead (com auditoria e replay).

---

## Conceitos-chave

### Multi-tenant (host-based)
A EchoLead foi desenhada para operar com **múltiplos tenants** de forma isolada, com:
- Identificação por **domínio/host** (ex.: `tenant-a.landing...`, `tenant-a.api...`)
- **Row-level tenancy** no banco (`tenant_id`)
- Autenticação via **JWT** com contexto do tenant

### Analytics-first
Dado operacional e dado analítico coexistem:
- **PostgreSQL** para dados operacionais e configurações.
- **ClickHouse** para analytics e exploração de eventos em escala.
- Estrutura preparada para “Datalake histórico” (raw/enriched).

### Event-driven e observável
- Ingestão → Enriquecimento → Regras → Saídas como pipeline de eventos.
- **Logs/metrics/tracing** para visão operacional real.
- Eventos imutáveis como fonte de verdade.

---

## Macro arquitetura (alto nível)

- **Tracking Script / Widget**
  - Captura parâmetros e gera `tracking_id`
  - Envia eventos (page_view, chat_start, form_submit, etc.)

- **API Gateway**
  - Autenticação, rate limit, roteamento e tenancy

- **Core Services**
  - **Ingestion**: recebe eventos
  - **Processing**: enriquece (utm, origem, device, fingerprint, etc.)
  - **Rules/Orchestration**: aplica regras e decide saídas
  - **Output**: envia para Chatwoot/CRM/Datalake

- **Storage**
  - PostgreSQL (operacional)
  - ClickHouse (analytics)
  - Replay/audit via eventos imutáveis

- **Integrações**
  - Chatwoot (conversa e atendimento)
  - CRM Sync (HubSpot/Pipedrive/Salesforce/etc. — via conectores)

---

## Principais resultados esperados

- **+ Conversão**: atendimento com contexto, menos atrito, mais fechamento.
- **- Perda de tracking**: UTMs e click_ids persistidos corretamente.
- **- Retrabalho**: menos “caça ao lead”, menos “qual campanha foi?”.
- **+ Governança**: auditoria e replay de eventos para correção e evolução.
- **+ Escala**: multi-tenant e pipeline event-driven pronto para crescer.

---

## Status do projeto (visão geral)

- Estrutura pensada para ambiente “prod-like” mesmo em dev:
  - Domínios por tenant via host-based routing (ex.: Traefik)
  - Chatwoot compartilhado com isolamento por inbox/tenant
  - API com tenancy por domínio + JWT
- Próximos passos típicos:
  - Consolidar tracking script + persistência de parâmetros
  - Pipeline de eventos (ingest/process/output) com fila
  - Console (KPIs + health + replay)
  - CRM Sync idempotente

---

## Glossário rápido

- **Tenant**: um cliente/conta isolada (ex.: “cliente A”, “cliente B”).
- **tracking_id**: identificador persistente do visitante/lead ao longo da jornada.
- **UTM/gclid/fbclid**: parâmetros de campanha e atribuição.
- **Replay**: reprocessar eventos históricos para corrigir/enriquecer novamente.
- **Idempotência**: garantir que sync/ações repetidas não criem duplicatas.

---

## License
Defina aqui (ex.: Proprietary / MIT / Apache-2.0).

---

## Contato
Se quiser contribuir, sugerir features ou discutir integrações (Chatwoot/CRMs), abra uma issue ou entre em contato pelo maintainer do repositório.






