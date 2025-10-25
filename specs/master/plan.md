# Implementation Plan: Licitaciones Inteligentes

**Branch**: `1-licitaciones-inteligentes` | **Date**: 2025-10-24 | **Spec**: specs/1-licitaciones-inteligentes/spec.md
**Input**: Feature specification from `specs/1-licitaciones-inteligentes/spec.md`

## Summary
Centralizar búsquedas SECOP I/II y proporcionar una calificación inteligente (0–100%) entre empresas y pliegos. Implementación basada en arquitectura de microservicios: servicios API en Python (FastAPI + Uvicorn), procesamiento de documentos con PyMuPDF → Pandas para extracción y matching, tareas asíncronas con Redis+Celery, motor de IA (Gemini API) para NER y análisis de pliegos, y almacenamiento en PostgreSQL con cifrado a nivel de campo para PII.

## Technical Context
**Language/Version**: Python 3.11  
**Primary Dependencies**: FastAPI, Uvicorn, Pydantic, Pandas, NumPy, PyMuPDF, Requests, Redis, Celery, SQLAlchemy (alterna), psycopg2, NetworkX, SlowAPI (rate limit), PyJWT/fastapi-jwt-auth, Gemini API client  
**Storage**: PostgreSQL for primary data; Redis for cache and Celery broker; object storage for documentos (S3-compatible) with server-side encryption.  
**Testing**: pytest for unit/integration, tox optional, GitHub Actions for CI/CD.  
**Target Platform**: Linux server (containers). Local dev via Docker Compose; initial deploy on Heroku/Vercel for web, migrate to AWS for prod.  
**Project Type**: Web backend microservices + lightweight frontend/UI  
**Performance Goals**: Endpoints críticos (login, consulta) < 3s p95; Búsquedas SECOP (aggregated) < 10s p95; Matching engine aim: interactive matching <1s for in-memory comparisons; heavy matching via async batch.  
**Constraints**: External SECOP API latency and rate limits; aim for privacy-by-design (PII encrypted, consent logs); no cloud lock-in; cost-sensitive for freemium tier.  
**Scale/Scope**: Initial MVP: thousands of users, tens of thousands of documents; design for horizontal scaling.

## Constitution Check
*GATE: Must be reviewed by maintainers/security before Phase 0 research.*

- Constitution requirement: Endpoints basic (login, consulta) MUST return <2s under normal conditions (Constitution IV).
- Planned SLOs in this feature: endpoints <3s; searches <10s.

Assessment: This differs from the constitution's stricter 2s requirement for endpoints. Rationale for deviation: SECOP external API variability and the decision to support more complex matching and document analysis in-phase 1 make a strict 2s p95 for all operations impractical without increased engineering cost. Mitigations proposed:
- Cache SECOP results and prefetch frequent queries to reduce perceived latency.
- Expose async flow for heavy analysis and surface immediate eligibility results using cached heuristics (<2s where possible).
- Instrument SLOs and create monitoring/alerts; require explicit approval from constitution maintainers to accept endpoints <3s for this feature.

Gate result: VIOLATION (SLO divergence). Action required: Open PR documenting the justification and mitigation and obtain approval from at least one maintainer and security/privacy reviewer before proceeding.

## Project Structure

```text
backend/
├── services/
│   ├── api/                 # FastAPI services (auth, search, score, documents)
│   ├── worker/              # Celery tasks (parsing, batch matching)
│   └── matcher/             # Matching engine library (pandas/numpy)
├── libs/
│   └── enterprise_utils/    # encryption, consent, audit helpers
├── contracts/
│   └── openapi.yaml
└── tests/
    ├── unit/
    └── integration/

infra/
├── docker-compose.yml
├── k8s/ (future)
└── ci/
```

**Structure Decision**: Use a backend-first microservices layout under `backend/services/` to isolate responsibilities and scale components independently (API vs worker vs matcher). Keep contracts in repo under `contracts/` and spec docs under `specs/1-licitaciones-inteligentes/`.

## Complexity Tracking
No additional constitution violations other than SLO divergence. No other high-complexity patterns required at this stage.

## Phase 0: Outline & Research
Create `research.md` capturing decisions and remaining technical unknowns. Research tasks (examples):
- Task: Validate SECOP API rate limits and pagination patterns.
- Task: Evaluate Gemini API cost/throughput and latency for bulk NER on pliegos.
- Task: Best practices for PyMuPDF → Pandas structured extraction of legal PDFs.
- Task: Encryption-at-rest design for per-field PII in PostgreSQL (pgcrypto or application-layer encryption).
- Task: Matching performance patterns with Pandas/NumPy vs specialized index (FAISS) for scaling.

(Research artifacts will be written to `specs/master/research.md`.)

## Phase 1: Design & Contracts (deliverables)
- `data-model.md` (entities, fields, validations, relationships)
- `contracts/openapi.yaml` (key endpoints: auth, search, score, upload, documents, admin)
- `quickstart.md` (how to run locally: Docker Compose with Postgres, Redis, backend services)

## Phase 1: Agent context update
Will run agent update script to refresh copilot context with new tech decisions.

---

Next steps:
- Generate `research.md`, `data-model.md`, `contracts/openapi.yaml`, `quickstart.md` in the implementation folder.
- Run `.specify/scripts/powershell/update-agent-context.ps1 -AgentType copilot` to update agent context.
