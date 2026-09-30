# MARS — Multi-Agent Reasoning System

A multi-agent AI decision system built with **LangGraph**, **LangChain**, **Supabase (pgvector)**, and **FastAPI**, with **Open WebUI** as the chat frontend.

Given a strategic business query, MARS fans out to four specialist agents (Finance, R&D, Legal, Operations) in parallel — each grounding its answer in similar historical cases retrieved from Supabase and, where relevant, live tool calls — then aggregates the four assessments into a single recommendation with a full explainability trail.

```
User Query
    │
    ▼
┌──────────┐
│  Master  │  (entry / router node)
└──────────┘
    │ fan-out (parallel)
    ├──────────────┬──────────────┬──────────────┐
    ▼              ▼              ▼              ▼
┌─────────┐  ┌─────────┐  ┌─────────┐  ┌────────────┐
│ Finance │  │   R&D   │  │  Legal  │  │ Operations │
└─────────┘  └─────────┘  └─────────┘  └────────────┘
    │              │              │              │
    └──────────────┴──────────────┴──────────────┘
                        │ fan-in
                        ▼
                 ┌────────────┐
                 │ Aggregator │  (ranks by confidence, resolves conflicts, explains)
                 └────────────┘
                        │
                        ▼
                  Final Decision
```

---

## Project Structure

- **[`MARS-confidence-implementation-and-dynamic-routing/`](./MARS-confidence-implementation-and-dynamic-routing)**: Core implementation including confidence metrics, dynamic gating router, backend services, dataset generators, and evaluation benchmarks.
  - **`backend/`**: FastAPI + LangGraph multi-agent architecture and OpenAI-compatible API endpoints.
  - **`dataset/`**: Corporate decision datasets and historical ground truth cases.
  - **`evaluation/`**: Benchmarking, backtesting, and conformal risk evaluation scripts.
  - **`ARCHITECTURE_CONFIDENCE_AND_ROUTING.md`**: Architectural breakdown of confidence estimation and dynamic routing.
  - **`TIER_1_IMPLEMENTATION_AND_BENCHMARKS.md`**: Formal benchmark specifications and validation protocols.
  - **`MARS_SYSTEM_DIAGRAMS_AND_SCHEMAS.md`**: UML system architecture diagrams and state machine specifications.

---

## Quick Start (Docker)

```bash
cd MARS-confidence-implementation-and-dynamic-routing/backend
cp .env.example .env   # configure GROQ_API_KEY, SUPABASE_URL, SUPABASE_KEY
docker compose up --build
```
