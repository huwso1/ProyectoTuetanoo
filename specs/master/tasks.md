# Tasks: Licitaciones Inteligentes

Feature: Licitaciones Inteligentes
Spec: specs/1-licitaciones-inteligentes/spec.md
Plan: specs/master/plan.md
Created: 2025-10-24

PHASE 1 — Setup (project initialization)

- [ ] T001 Create repository backend layout and base python project files in backend/services/api (pyproject.toml, README, .env.example)
- [ ] T002 Create Docker Compose for dev with services: postgres, redis, backend, worker, object-storage in infra/docker-compose.yml
- [ ] T003 Initialize CI workflow file for GitHub Actions at .github/workflows/ci.yml (run lint, pytest, build docker)
- [ ] T004 Add initial requirements file backend/services/api/requirements.txt listing FastAPI, Uvicorn, Pydantic, SQLAlchemy, psycopg2, Pandas, NumPy, PyMuPDF, Requests, Redis, Celery, PyJWT, fastapi-jwt-auth, SlowAPI, networkx
- [ ] T005 Create quickstart instructions at specs/master/quickstart.md (ensure docker-compose commands match repo layout)

PHASE 2 — Foundational (blocking prerequisites)

- [ ] T006 Initialize database migrations: add Alembic config at backend/services/api/alembic.ini and migrations/ directory
- [ ] T007 Implement base models and ORM layer in backend/services/api/src/models/base.py (SQLAlchemy engine, session, Base)
- [ ] T008 Implement Usuario and Empresa models in backend/services/api/src/models/user.py and backend/services/api/src/models/company.py per data-model.md fields
- [ ] T009 Add encryption helper and key management scaffolding in backend/services/libs/enterprise_utils/encryption.py (application-layer encryption hooks)
- [ ] T010 Implement consent and audit logging helpers in backend/services/libs/enterprise_utils/consent_audit.py
- [ ] T011 Configure Redis and Celery integration in backend/services/worker/celery_app.py and add example task ping
- [ ] T012 Create object storage adapter interface in backend/services/api/src/storage/s3_adapter.py (S3-compatible URL save/retrieve)
- [ ] T013 Create DB migration to add tables for Usuario, Empresa, Proyecto, Documento, Score in migrations/versions/
- [ ] T014 Add JWT-based auth skeleton and middleware in backend/services/api/src/auth/*.py using fastapi-jwt-auth
- [ ] T015 Add rate limiting middleware scaffold with SlowAPI in backend/services/api/src/middleware/rate_limit.py

PHASE 3 — User Story 1 (US1) — Búsqueda y calificación (MiPyme) [P1]
Goal: Allow MiPyme to search SECOP by UNSPSC/ubicación and see compatibility scores + checklist.
Independent test criteria: API returns aggregated SECOP results with score and checklist for a sample query.

- [ ] T016 [US1] Implement SECOP ingestion client (requests wrapper) in backend/services/api/src/integrations/secop_client.py
- [ ] T017 [US1] Implement search endpoint in backend/services/api/src/api/search.py with path /search and query param q (map to SECOP client)
- [ ] T018 [US1] Implement caching layer for search results in backend/services/api/src/cache/search_cache.py using Redis
- [ ] T019 [US1] Implement matching service entrypoint in backend/services/matcher/service.py that accepts pliego data + company profile and returns score,gaps,confidence
- [ ] T020 [P] [US1] Implement Score model persistence in backend/services/api/src/models/score.py and repository in backend/services/api/src/repositories/score_repo.py
- [ ] T021 [US1] Wire search flow: search endpoint calls secop_client -> caches results -> triggers lightweight matching via matcher.service for immediate heuristics (implement in backend/services/api/src/api/search.py)
- [ ] T022 [US1] Create acceptance test scaffold in tests/integration/test_search_score.py referencing sample SECOP fixture (if tests requested) at tests/integration/test_search_score.py

PHASE 4 — User Story 2 (US2) — Análisis rápido (Consultor) [P2]
Goal: Allow consultant to paste/upload pliego fragments and get summarized issues and suggestions.
Independent test criteria: API returns bullet summary and at least 3 suggestions for a provided pliego fragment.

- [ ] T023 [US2] Implement documents upload endpoint in backend/services/api/src/api/documents.py (/documents/upload) to accept file and metadata
- [ ] T024 [US2] Implement PyMuPDF parsing worker task in backend/services/worker/tasks/parse_pdf.py that converts PDF -> structured JSON and stores parsed_structure in Documento
- [ ] T025 [US2] Implement Gemini NER/summarization integration wrapper in backend/services/api/src/integrations/gemini_client.py
- [ ] T026 [US2] Implement pliego summarization endpoint in backend/services/api/src/api/analyze.py that accepts raw text or document id and returns NER bullets and suggestions
- [ ] T027 [US2] Add unit tests for PDF parsing logic in tests/unit/test_parsing.py

PHASE 5 — User Story 3 (US3) — Seguimiento de documentos (checklists & RUP alerts) [P1]
Goal: Track required documents, upload, and alert on RUP expiry.
Independent test criteria: Upload document, checklist marks completed items, alerts scheduled for expirations.

- [ ] T028 [US3] Implement Documento model fields and parsed_structure storage in backend/services/api/src/models/document.py
- [ ] T029 [US3] Implement checklist generator for a pliego in backend/services/matcher/checklist.py that maps requirements to required document types
- [ ] T030 [US3] Implement scheduled alert tasks in backend/services/worker/tasks/alerts.py to send notifications 30/7/1 days before RUP expiry (hook to notification service)
- [ ] T031 [US3] Create API endpoint to view checklist and upload documents per project in backend/services/api/src/api/projects.py (/projects/{id}/checklist)

PHASE 6 — User Story 4 (US4) — Asistente investigación y presupuesto [P3]
Goal: Provide basic budget estimate and market references.
Independent test criteria: Endpoint returns budget range and 2 reference factors for sample request.

- [ ] T032 [US4] Implement assistant endpoint in backend/services/api/src/api/assistant.py that accepts a project context and returns budget range and references
- [ ] T033 [US4] Integrate assistant with Gemini client for market reference extraction in backend/services/api/src/integrations/gemini_client.py
- [ ] T034 [US4] Add simple confidence tagging logic to responses (alta/media/baja) in backend/services/api/src/services/confidence.py

PHASE 7 — User Story 5 (US5) — API pública (integration) [P1]
Goal: Expose authenticated endpoints for external clients to query score and process state.
Independent test criteria: Authenticated API returns JSON with score, warnings and suggestions for a provided request.

- [ ] T035 [US5] Implement /score endpoint in backend/services/api/src/api/score.py accepting company and pliego identifiers, returning stored or computed score
- [ ] T036 [P] [US5] Implement API authentication and authorization checks in backend/services/api/src/auth/deps.py (JWT token validation)
- [ ] T037 [US5] Create API documentation OpenAPI file at backend/services/contracts/openapi.yaml and sync with specs/master/contracts/openapi.yaml

PHASE 8 — Polish & Cross-Cutting Concerns

- [ ] T038 Add observability: implement metrics for latency, error rate, confidence rates in backend/services/api/src/observability/metrics.py
- [ ] T039 Implement logging and structured error types that do not leak PII in backend/services/api/src/logging_config.py
- [ ] T040 Add CI tests for contract verification that OpenAPI matches implemented endpoints at .github/workflows/contract_check.yml
- [ ] T041 Implement rate limiting policies for freemium tier in backend/services/api/src/middleware/rate_limit.py (SlowAPI rules)
- [ ] T042 Prepare deployment manifest examples for Heroku and AWS in infra/deploy/ (heroku.yml, k8s placeholders)

Dependencies (story completion order)

1. Setup (T001..T005)
2. Foundational infra and models (T006..T015)
3. US1 (T016..T022)
4. US3 (T028..T031) — document tracking helps matching accuracy
5. US5 (T035..T037) — external API depends on models and matching
6. US2 (T023..T027) — consultant flows reuse parsing and gemini integration
7. US4 (T032..T034)
8. Polish (T038..T042)

Parallel opportunities

- [P] Tasks marked [P] can be executed in parallel (examples): T020, T036, T019, T041
- Setup tasks T001..T005 are independent and parallelizable across contributors
- Worker tasks (T024, T030) can be implemented while API endpoints are built (parallel)

MVP suggestion

- Suggested MVP scope: US1 minimal pipeline + Foundational tasks
  - Minimal tasks: T001-T015, T016-T021, T020 (persistence), T038 (basic metrics)

Summary

- Total tasks: 42
- Tasks per story: US1=7, US2=5, US3=4, US4=3, US5=3, Setup=5, Foundational=10, Polish=5
- Parallel opportunities identified: multiple [P] tasks for independent implementation

Validation

- All tasks follow the checklist format and include file paths. Each user story phase contains independent test criteria for acceptance.
