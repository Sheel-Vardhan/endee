# Endee RAG + Semantic Search Demo (Python)

This example is a complete **ingest → embed → index → search → RAG** pipeline where **Endee is the vector database**.

## What you get

- **Semantic Search**: query your docs using vector similarity via Endee.
- **RAG**: retrieve top-k chunks from Endee and generate an answer (optional LLM).
- **Filters**: store simple filter fields (e.g., `source`, `doc_id`) and query with filters.

## Prerequisites

- Docker + Docker Compose
- Python 3.10+

## 1) Start Endee (vector DB)

From the repository root:

```bash
docker compose -f examples/rag_semantic_search/docker-compose.endee.yml up -d
```

Endee Dashboard: `http://localhost:8080`

## 2) Setup Python env

From `examples/rag_semantic_search/`:

```bash
python -m venv .venv
.\.venv\Scripts\activate
pip install -r requirements.txt
copy env.example .env
```

## 3) Ingest sample docs into Endee

```bash
python -m app.cli ingest --path data/sample_docs
```

## 4) Run API (Semantic Search + RAG)

```bash
python -m app.cli api --host 127.0.0.1 --port 8000
```

Try:

- `GET http://127.0.0.1:8000/health`
- `POST http://127.0.0.1:8000/search` with JSON: `{"query":"what is Endee?", "top_k": 5}`
- `POST http://127.0.0.1:8000/ask` with JSON: `{"query":"How do I run Endee with docker?", "top_k": 5}`

## Notes

- This demo uses `sentence-transformers/all-MiniLM-L6-v2` (dimension **384**) so the Endee index is created with `dimension=384` and `space_type=cosine`.
- If you enable Endee authentication (`NDD_AUTH_TOKEN`), set the same token in `.env` (`ENDEE_AUTH_TOKEN=...`).

