TruthOS

AI-powered, evidence-based fact verification platform. Modular monolith built with Clean Architecture and DDD, designed to extract claims, link evidence, and verify facts with full citation tracking via a knowledge graph.

Status: Foundational architecture + authentication/identity module complete and tested. Claim extraction, evidence retrieval, verification, and knowledge graph integration are in progress.

Tech Stack

Backend

FastAPI, Python
SQLAlchemy 2 (async) + Alembic
Pydantic v2
PostgreSQL (module-namespaced schemas)
Redis (caching, rate limiting)
Neo4j (knowledge graph)
Argon2 (password hashing) + JWT (rotating refresh tokens)

Frontend

Next.js 15, TypeScript
Tailwind CSS
Zustand (state)
Axios (API client with refresh-on-401 interceptor)

AI / ML

Qdrant (vector database)
Embeddings: Voyage AI (primary), self-hosted BGE-large (fallback)
LLM inference: Anthropic/OpenAI APIs + self-hosted vLLM
Custom AI orchestration (no LangChain — direct, typed pipeline)
Braintrust (prompt evaluation/versioning)

Security

httpOnly refresh-token cookies
Double-submit-cookie CSRF protection
Security headers middleware
Redis-backed rate limiting

Infrastructure

Docker Compose v2 (network-segmented, dev/prod targets, health checks)
Nginx reverse proxy

Architecture

Clean Architecture, Domain-Driven Design, CQRS
Repository pattern, Transactional Outbox, Dependency Injection
Ten bounded contexts: ingestion, content processing, claim extraction, evidence retrieval, knowledge graph, memory, verification, query answering, identity/access, notifications, billing
Getting Started
bash
cp .env.example .env
# set JWT_SECRET_KEY in .env — generate with:
python3 -c "import secrets; print(secrets.token_urlsafe(48))"

docker compose up --build
Frontend: http://localhost:3000
Backend API docs: http://localhost:8000/docs
Health check: http://localhost:8000/health/ready
Project Structure
truthos/
├── backend/        FastAPI app — core, models, schemas, repositories, services, api/v1
├── frontend/        Next.js app — app router, components, lib
├── nginx/              reverse proxy config
└── docker-compose.yml
Testing
bash
cd backend
python -m venv .venv && source .venv/bin/activate
pip install -r requirements-dev.txt
python -m pytest tests/ -v

22 automated tests covering registration, login, RBAC, refresh-token rotation, CSRF protection, and security headers — all passing.

License

TBD
