# Research: Licitaciones Inteligentes (Phase 0)

## Unknowns and Tasks
- SECOP API availability, authentication, rate limits, pagination and data formats.
- Gemini API throughput, latency, cost per request for NER and summarization.
- Best practices for parsing legal PDF pliegos with PyMuPDF and structuring into Pandas DataFrame.
- Encryption-at-rest options for per-field PII in PostgreSQL (pgcrypto vs application-layer encryption).
- Matching engine scale: Pandas/NumPy vs using vector indexes (FAISS) for semantic matching.

## Decisions (initial)
- Decision: Use FastAPI + Uvicorn for backend microservices (Python 3.11). Rationale: fast development, ecosystem compatibility with Pydantic and async.
- Decision: Use PyMuPDF + Pandas for structured parsing of PDFs; research to validate edge cases with malformed PDFs.
- Decision: Use Gemini API for NER and pliego summarization; research to validate cost and latency.
- Decision: Use PostgreSQL as primary datastore and Redis as Celery broker/cache.

## Next steps
- Execute research tasks and consolidate in this document.
