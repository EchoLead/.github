<p align="center">
  <img src="./el.png" alt="EchoLead banner" width="720" />
</p>

# EchoLead

**EchoLead** is an **event-driven, multi-tenant, and analytics-first** platform for **lead capture, enrichment, and activation** — connecting **Landing Pages + Chat (Chatwoot) + Console + Integrations (CRM)** in a single, auditable flow prepared for scale.

The focus is to solve the gap between **marketing → conversation → conversion → CRM**, ensuring that **every lead arrives with complete context**, with **end-to-end traceability** and consistent data for operations and analysis.

## Why EchoLead exists

Marketing and sales teams typically struggle with:

- **Loss of UTMs / gclid / fbclid** between pages, redirects, and chat initiation.
- **Conversations without context** (origin, campaign, landing, referrer, device), resulting in "blind" service.
- **Fragmented data**: GA4/Pixel says one thing, CRM another, chat another.
- **Fragile integrations**: lead duplication, sync failures, lack of idempotency.
- **Difficulty with auditing**: "where did this lead come from?", "which campaign generated it?", "why was it routed to X?".
- **Poor operational visibility**: no real funnel, no health checks, and no replay.

EchoLead was born to **centralize lead intelligence**: capture, persist, enrich, apply rules, and sync with destinations (Chat, CRM, Data Warehouse), maintaining an **immutable history** for auditing and reprocessing.

## What EchoLead solves (in practice)

### 1) Complete lead context (from the first click)

- Persistence of **tracking_id** and parameters (**UTM, gclid, fbclid, referrer, page_url**).
- Context continuity between **landing → navigation → chat → conversion**.
- Metadata ready for automations (routing, scoring, segmentation).

### 2) Chat with intelligence (Chatwoot)

- Chat widget receives the lead already identified and with context.
- Enables:
    - Inbox per tenant / client
    - Automatic labels/tags
    - Rule-based routing
    - Consistent history (no "lost lead")

### 3) Robust and auditable data pipeline

- **Event-driven** architecture with **queue/messaging**.
- Storage of **raw and enriched events**.
- **Replay** and **audit** through immutable history (reprocess rules without losing the original).

### 4) Operational console (control plane)

- KPIs and funnel per tenant: volume, origin, campaigns, conversion, SLA, integration status.
- Diagnostics: health, queues, errors, "stuck" events.
- Actions: event replay, re-sync, lead/conversation inspection.

### 5) Reliable CRM synchronization

- **Idempotent** sync (avoids duplication).
- Propagates **origin/campaign/conversation** to CRM.
- Failure handling with retry and traceability.

## Who it's for

- **Agencies** and multi-client operations that need to standardize tracking and lead delivery.
- **SaaS / products** with multiple tenants, each with their own landings and channels.
- **Growth + SDR/closer** teams that need context in service and reliable data in CRM.
- Operations that want a **"single source of truth"** for leads (with audit and replay).

## Key concepts

### Multi-tenant (host-based)

EchoLead was designed to operate with **multiple tenants** in isolation, with:

- Identification by **domain/host** (e.g., `tenant-a.landing...`, `tenant-a.api...`)
- **Row-level tenancy** in the database (`tenant_id`)
- Authentication via **JWT** with tenant context

### Analytics-first

Operational data and analytical data coexist:

- **PostgreSQL** for operational data and configurations.
- **ClickHouse** for analytics and exploration of events at scale.
- Structure prepared for "historical Datalake" (raw/enriched).

### Event-driven and observable

- Ingestion → Enrichment → Rules → Outputs as event pipeline.
- **Logs/metrics/tracing** for real operational visibility.
- Immutable events as source of truth.

## Macro architecture (high level)

- **Tracking Script / Widget**
    - Captures parameters and generates `tracking_id`
    - Sends events (page_view, chat_start, form_submit, etc.)
- **API Gateway**
    - Authentication, rate limit, routing, and tenancy
- **Core Services**
    - **Ingestion**: receives events
    - **Processing**: enriches (utm, origin, device, fingerprint, etc.)
    - **Rules/Orchestration**: applies rules and decides outputs
    - **Output**: sends to Chatwoot/CRM/Datalake
- **Storage**
    - PostgreSQL (operational)
    - ClickHouse (analytics)
    - Replay/audit via immutable events
- **Integrations**
    - Chatwoot (conversation and service)
    - CRM Sync (HubSpot/Pipedrive/Salesforce/etc. — via connectors)

## Main expected results

- **+ Conversion**: service with context, less friction, more closing.
- **- Loss of tracking**: UTMs and click_ids correctly persisted.
- **- Rework**: less "lead hunting", less "which campaign was it?".
- **+ Governance**: audit and replay of events for correction and evolution.
- **+ Scale**: multi-tenant and event-driven pipeline ready to grow.

## Project status (overview)

- Structure designed for "prod-like" environment even in dev:
    - Domains per tenant via host-based routing (e.g., Traefik)
    - Shared Chatwoot with isolation per inbox/tenant
    - API with tenancy by domain + JWT
- Typical next steps:
    - Consolidate tracking script + parameter persistence
    - Event pipeline (ingest/process/output) with queue
    - Console (KPIs + health + replay)
    - Idempotent CRM Sync

## Quick glossary

- **Tenant**: an isolated client/account (e.g., "client A", "client B").
- **tracking_id**: persistent identifier of the visitor/lead throughout the journey.
- **UTM/gclid/fbclid**: campaign and attribution parameters.
- **Replay**: reprocess historical events to correct/enrich again.
- **Idempotency**: ensure that repeated sync/actions don't create duplicates.

## License

Proprietary — All rights reserved.

EchoLead is proprietary software. Copying, modifying, distributing, or using this software, by any means, without prior written authorization from EchoLead is prohibited.

For questions about licensing, commercial use, or platform access, contact the maintainers of this repository.

### Third-party components

EchoLead integrates and operates third-party open-source components (e.g., Chatwoot) under their respective licenses.

These components are **not** covered by this Proprietary license. Consult the official repositories for applicable terms.
