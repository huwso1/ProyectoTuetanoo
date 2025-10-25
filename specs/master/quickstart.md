# Quickstart: Licitaciones Inteligentes (local)

Prereqs: Docker, Docker Compose

1. Copy `.env.example` to `.env` and set database and Gemini API credentials.
2. Start services:
   docker-compose up -d --build
3. Run migrations:
   docker compose exec backend alembic upgrade head
4. Start worker:
   docker compose exec worker celery -A worker.app worker --loglevel=info
5. API available at http://localhost:8000
