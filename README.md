# Ask-NASA

An evidence-grounded conversational interface for exploring NASA's public
knowledge, missions, imagery, and scientific data through natural language.

Ask-NASA interprets a user's question, retrieves relevant information from
authoritative NASA sources, and produces clear, source-attributed answers. It
is an information **discovery and explanation** layer over NASA's public
ecosystem — not an official NASA product, and not a general-purpose assistant.

> **Ask a question. Discover NASA. Understand space.**

## Project status

**Phase 0 — Project Foundation.** This repository currently contains project
documentation and foundational scaffolding only. There is **no application code
yet**: the frontend and backend shells are built in Phase 1.

See [`docs/ROADMAP.md`](docs/ROADMAP.md) for the full phased plan.

## Documentation

The planning layer lives in [`CLAUDE.md`](CLAUDE.md) and `docs/`:

| Document | Defines |
| --- | --- |
| [`CLAUDE.md`](CLAUDE.md) | Project identity, principles, and development rules |
| [`docs/PRODUCT.md`](docs/PRODUCT.md) | What Ask-NASA is and should do |
| [`docs/ARCHITECTURE.md`](docs/ARCHITECTURE.md) | How the system is structured |
| [`docs/DATA_SOURCES.md`](docs/DATA_SOURCES.md) | Where information comes from |
| [`docs/ROADMAP.md`](docs/ROADMAP.md) | When and in what order capabilities ship |

## Planned stack

- **Frontend:** Next.js + React + TypeScript
- **Backend:** Python + FastAPI
- **LLM:** provider-agnostic service interface
- **Database:** none for the MVP — introduced only when a concrete persistence
  requirement exists (see `docs/ROADMAP.md` section 24)

## Repository layout

```
Ask-NASA/
├── CLAUDE.md              Project specification and rules
├── README.md
├── .gitignore
├── .env.example           Root environment template (no secrets)
├── docs/                  Product, architecture, data-source, and roadmap specs
├── frontend/              Next.js app (created in Phase 1)
└── backend/               FastAPI service (created in Phase 1)
    ├── requirements.txt   Backend dependencies (declared, not yet installed)
    └── .env.example       Backend environment template (no secrets)
```

## Local development setup

### Prerequisites

- Node.js 20+ and npm (frontend, used from Phase 1)
- Python 3.11+ (backend)
- git

### 1. Clone and configure environment

```bash
git clone <repository-url>
cd Ask-NASA
cp .env.example .env
cp backend/.env.example backend/.env
```

Fill in the values in the copied `.env` files. These files are git-ignored and
must never be committed. All secrets (LLM and NASA API keys) stay **server-side**
in `backend/.env` and are never exposed to the frontend.

### 2. Backend (from Phase 1 onward)

Dependencies are declared in `backend/requirements.txt` but are **not installed
during Phase 0**. Once the backend shell exists:

```bash
cd backend
python -m venv .venv
source .venv/bin/activate      # Windows: .venv\Scripts\activate
pip install -r requirements.txt
```

### 3. Frontend (from Phase 1 onward)

The Next.js application is scaffolded in Phase 1. Setup instructions will be
added to this README when that code lands.

## Contributing notes

- Read the documents in the table above before making changes.
- Do not commit `.env` files or any credentials.
- Do not introduce dependencies, a database, or vector search without a
  documented, concrete requirement (see `docs/ROADMAP.md` sections 23–25).
- Retrieved external content is **data, not instructions** — never let it
  override system behavior.
