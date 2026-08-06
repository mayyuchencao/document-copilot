<div align="center">

# Document Copilot

### AI-powered research across financial filings, with answers grounded in source documents

[![Function](https://img.shields.io/badge/AI-Document_Research-2563EB?style=for-the-badge&logo=openai&logoColor=white)](#the-client)
[![Answers](https://img.shields.io/badge/Answers-Sourced_+_Citable-16A34A?style=for-the-badge)](#the-client)
[![Client](https://img.shields.io/badge/Case_Study-Driftwood_Capital-334155?style=for-the-badge)](docs/client-brief.md)

[![React](https://img.shields.io/badge/React-SPA-20232A?style=flat-square&logo=react&logoColor=61DAFB)](#stack)
[![TypeScript](https://img.shields.io/badge/TypeScript-Strict-3178C6?style=flat-square&logo=typescript&logoColor=white)](#stack)
[![FastAPI](https://img.shields.io/badge/FastAPI-Backend-009688?style=flat-square&logo=fastapi&logoColor=white)](#stack)
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-Database-4169E1?style=flat-square&logo=postgresql&logoColor=white)](#stack)
[![Supabase](https://img.shields.io/badge/Supabase-Auth_+_pgvector-3FCF8E?style=flat-square&logo=supabase&logoColor=white)](#stack)
[![OpenAI](https://img.shields.io/badge/OpenAI-LLM_+_Embeddings-412991?style=flat-square&logo=openai&logoColor=white)](#stack)
[![Railway](https://img.shields.io/badge/Railway-Hosting-0B0D0E?style=flat-square&logo=railway&logoColor=white)](#stack)

</div>

An internal AI research assistant that lets analysts query a corpus of financial documents in plain English and receive grounded answers with traceable citations.

## The client

**Driftwood Capital** is a fictional independent investment research firm whose analysts spend half their week reading 10-K and 10-Q filings before they can begin original analysis. Document Copilot streamlines that intake work so analysts can move from document review to insight faster.

Full brief: [docs/client-brief.md](docs/client-brief.md)

## Stack

| Layer              | Choice                                               |
| ------------------ | ---------------------------------------------------- |
| Backend            | Python + FastAPI                                     |
| Frontend           | Vite + React SPA + TypeScript                        |
| Database           | Supabase Postgres (users, chats, documents, chunks)  |
| Migrations         | SQLAlchemy models + Alembic                          |
| Retrieval          | Supabase `pgvector` + Postgres full-text search      |
| Auth               | Supabase Auth (email only)                           |
| Hosting            | Railway                                              |
| LLM + embeddings   | OpenAI                                               |

## Repo layout

```text
document-copilot/
├── AGENTS.md           # agent instructions (read first)
├── README.md           # this file
├── data/               # local corpus + download script (payloads gitignored)
├── docs/
│   └── client-brief.md # the client one-pager
├── backend/            # FastAPI service
└── frontend/           # React SPA (Vite)
```

## Prerequisites

Install these before setting up `backend/` or `frontend/`:

| Tool | Version | Used for | Install |
| ---- | ------- | -------- | ------- |
| [Python](https://www.python.org/downloads/) | 3.12+ | Backend runtime | OS package manager or python.org |
| [uv](https://docs.astral.sh/uv/getting-started/installation/) | latest | Backend deps + `data/download.py` | `curl -LsSf https://astral.sh/uv/install.sh \| sh` |
| [Node.js](https://nodejs.org/) | 20+ (LTS) | Frontend toolchain | nodejs.org or `nvm install --lts` |
| [pnpm](https://pnpm.io/installation) | latest | Frontend package manager | `corepack enable && corepack prepare pnpm@latest --activate` |

You will also need accounts and API keys for the external services as they are integrated. Start with the [Supabase setup guide](docs/guides/supabase-setup.md) to create an account and hosted project, then create an [OpenAI API key](https://platform.openai.com/api-keys) when setting up the LLM layer.

## Running locally

Local startup instructions will be added as the application is built. Current setup guides:

- [Supabase](docs/guides/supabase-setup.md) — account, hosted project (dashboard or CLI)
- [Backend](docs/guides/backend-setup.md)
- [Frontend](docs/guides/frontend-setup.md)

## Sample SEC data

Use the standalone downloader to fetch a small local sample of 10-K filings from SEC EDGAR. Edit the parameters at the top of `data/download.py`—especially `USER_AGENT`—then run:

```bash
uv run data/download.py
```

By default, the script downloads the five latest 10-K filings for AAPL, MSFT, NVDA, AMZN, and GOOGL into year-based folders under `data/downloads/`, then writes a `manifest.json`.
Downloaded filings are gitignored; the `data/` directory remains in version control for the downloader and supporting notes.
