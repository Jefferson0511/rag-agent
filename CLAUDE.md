# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

A LangChain-powered RAG (Retrieval-Augmented Generation) agent. It ingests documents, chunks and embeds them into PgVector, and answers queries through an LLM using retrieved context. Built as a learning project to bridge skills for SWE/MLE roles.

## Teaching Mode

**Teaching mode is ON by default.** Always explain the concept and the "why" before writing code.

- `/build` — user wants code only, no explanations. Skip teaching, write the implementation.
- `/teach` — return to explaining first, then code.
- Never generate an entire module at once. Build one function or component at a time.
- After completing each component, ask the user to explain it back before moving on.

## Planned Stack

| Layer | Technology |
|---|---|
| Language | Python |
| RAG framework | LangChain |
| Vector store | PgVector (PostgreSQL + pgvector extension) |
| LLM | Configurable (OpenAI / local via Ollama) |
| Experiment tracking | MLflow |
| Containerization | Docker / Docker Compose |
| Frontend (later) | Next.js |

## Planned Architecture

```
documents/          # Raw input files (PDF, TXT, etc.)
src/
  ingestion/        # Document loading, chunking, embedding, upsert to PgVector
  retrieval/        # Vector similarity search, context assembly
  agent/            # LangChain agent/chain definition, prompt templates
  tracking/         # MLflow logging helpers
config/             # Environment and model configuration
docker/             # Dockerfiles and docker-compose.yml
tests/              # Unit and integration tests
```

**Data flow:**
1. Documents → loader → text splitter → embeddings → PgVector (ingestion pipeline)
2. User query → embedder → similarity search → retrieved chunks + query → LLM → answer (query pipeline)

## Development Commands

> Commands will be added here as the project is set up.

```bash
# Install dependencies (once pyproject.toml / requirements.txt exist)
pip install -r requirements.txt

# Start PostgreSQL + pgvector via Docker
docker compose up -d db

# Run ingestion pipeline
python -m src.ingestion.ingest --source documents/

# Run a query
python -m src.agent.query "What is ...?"

# Start MLflow UI
mlflow ui

# Run tests
pytest

# Run a single test
pytest tests/test_ingestion.py::test_chunk_documents -v
```

## Key Conventions

- Python for all backend and ML code.
- One responsibility per file — keep ingestion, retrieval, and agent logic in separate modules.
- Configuration (API keys, DB URLs, model names) via environment variables; use a `.env` file locally and never commit it.
- MLflow runs should be logged for each ingestion and query experiment so results are reproducible.
- Never list a technology in the stack unless it appears in at least one code file or is actively being used.

## Learner Notes

- Jefferson is an MS CS student learning agentic AI systems.
- Build incrementally — one function at a time, never an entire module at once.
- When teaching mode is ON, ask "can you explain what we just built?" after each component before proceeding.
- Preferred style: clean, readable Python with comments explaining WHY, not just what.
