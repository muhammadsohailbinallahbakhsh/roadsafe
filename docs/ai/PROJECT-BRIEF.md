# RoadSafe: Project Brief

Canonical copy. A copy also lives in the Claude Project "Road Accident Project" for
learning and strategy threads. If they differ, this file wins until the strategy thread
says otherwise.

## What we are building
An AI-based road accident prediction platform. It uses historical accident data plus
environmental data (weather, time, lighting, road features) to predict accident-prone
areas, explain why, and let people ask questions and act on the answers.

## Why
Two goals, equally important:
1. A portfolio project that reads as senior-engineer work: architecture, scale thinking,
   code quality, and real AI engineering (not a chatbot over PDFs).
2. Hands-on learning of: AI security and red teaming, Azure AI services, LLM evaluation
   and observability, RAG and agents, Azure cloud, CI/CD and IaC.

## Who it is for (the demo story)
A traffic authority. Two portals:
- Analyst/admin portal (internal): full detail, copilot, actions with approval.
- Public portal (read-only): aggregated, anonymised risk information.

## Features
1. **Prediction engine.** Accident records and environmental data are normalised onto an
   H3 hexagon grid. A classical ML model scores risk per cell and condition, with
   SHAP-based explanations, a versioned model registry, and a retrain pipeline that only
   promotes a model that beats the current one.
2. **Risk dashboard.** Interactive map and heatmap, filters (time, weather, road type),
   drill-down per cell with history, trend and plain-language "why".
3. **AI road-safety copilot.** Natural-language questions over the prediction engine.
   Retrieval-augmented answers grounded in a knowledge base of road-safety material,
   with source attribution. Agentic actions (draft an intervention recommendation, open a
   ticket in a mock downstream system) behind a human approval step.
4. **Trust and safety layer.** Prompt-injection and jailbreak defenses, structured-output
   validation, PII protection, permission-aware retrieval, least-privilege tool calling,
   and an automated red-team suite that runs in CI.
5. **Evaluation and observability.** Groundedness/relevance evals with CI gates, prompt
   versioning and regression tests, tracing across model, retrieval and tool calls, and
   token and cost monitoring.
6. **Engineering backbone.** Clean Architecture, DDD, CQRS, event-driven core with the
   outbox pattern, full test pyramid, containerised, deployed with IaC.

Stretch (not committed): driver-facing "is my route safe now", alerting when a zone
crosses a threshold, extracting the copilot as a real separate service.

## Locked decisions
| Area | Decision |
|---|---|
| Dataset | US Accidents (Kaggle). Licence terms to be checked by me before publishing. |
| Database | PostgreSQL with PostGIS and pgvector. Redis for caching. |
| Backend | ASP.NET Core on .NET 10, Clean Architecture, DDD, CQRS. Modular monolith. |
| Other deployables | Python FastAPI ML service, .NET worker, Azure Functions (required, not optional), React frontend |
| ML | LightGBM or XGBoost, SHAP, MLflow registry, H3 spatial index, Open-Meteo for live weather |
| Frontend | React, TypeScript, Vite, MapLibre GL, deck.gl |
| AI layer | Microsoft.Extensions.AI plus Semantic Kernel or Microsoft Agent Framework. Official C# MCP SDK. |
| Models | Ollama locally, GitHub Models for prototyping, Azure OpenAI for the final Azure burst |
| RAG | pgvector plus Postgres full-text search (hybrid), local reranker. Azure AI Search is the cloud swap. |
| Messaging | Azure Service Bus (emulator locally), outbox pattern |
| Local orchestration | Aspire |
| Observability | OpenTelemetry, then Application Insights |
| AI quality and safety | promptfoo, garak, PyRIT, Microsoft.Extensions.AI.Evaluation, Azure AI Content Safety |
| Auth | ASP.NET Core Identity first; Microsoft Entra ID introduced in Phase 0 on a hello endpoint and adopted in Phase 4 |
| Deploy and IaC | Docker, Azure Container Apps, Azure Functions, Key Vault, Bicep. AKS is a stretch only. |
| CI/CD and tests | GitHub Actions, xUnit, Testcontainers |
| Repo | One public repo named `roadsafe`. The `.sln` contains only .NET projects. Python and React live in their own folders. |

Open item: MediatR, MassTransit and FluentAssertions have licensing changes. I am
evaluating them. Do not add them until a decision is recorded.

## Constraints
- Azure budget cap of about $20-50 total. Local-first. Free tools where possible.
- Azure free account is NOT activated yet. Its $200 credit expires 30 days after
  activation, so activation is timed with my strategy thread. One account per person only.
- Demo project: it does not need to run continuously. Spin up, record, tear down.
- Pace: 10-15 hours per week. No hard deadline. The goal matters more than the date.
- Public repo with build-in-public content, so no secrets or personal data, ever.

## Repo layout
```
roadsafe/
  RoadSafe.sln         (.NET projects only)
  src/  tests/
  services/ml/         (Python)
  web/                 (React)
  infra/               (Bicep)
  docs/adr/            (my Architecture Decision Records)
  docs/ai/             (brief, roadmap, logs, change requests: Claude Code may write here)
  CLAUDE.md  .claude/
```

## Working agreement
- Three places work happens: Claude Code threads (build, one per step), learning threads
  in the Claude Project (new technology before a step), and the strategy thread in the
  Claude Project (decisions, change requests, phase planning).
- I write the code. Claude Code mentors, reviews and logs.
- **Change rule:** if Claude Code changes, extends, or suggests changing anything in this
  brief or the roadmap, it must say so and log it in `CHANGE-REQUESTS.md`. I bring it to
  the strategy thread and we decide together. Minor extensions inside a step may be
  applied and reported. Anything touching stack, architecture, phases, scope, security or
  cost waits for a decision.

## Out of scope
Real production traffic data, mobile apps, multi-tenant billing, Kubernetes in the core
build.
