# RoadSafe: Roadmap

How to read this: the table for each step says what to build, what to learn first, and how
to know it is done. Learn-first links marked (verified) were checked against current
Microsoft docs on 2026-10-30. Others are from memory, so check that they still open.

Claude Code may add detail under a step marked "(extension)" and must report it. Changing,
removing, reordering or adding steps or phases needs a decision (see CLAUDE.md, Change
control). Progress is tracked in `START-HERE.md`, not here.

## Phase 0: Cloud Walking Skeleton (about 3-4 weeks)
Steps 0.1 and 0.2 are free and local, so do them BEFORE activating Azure. Activation is
decided in the strategy thread before 0.3. Then finish 0.3 to 0.10 inside the 30-day
credit window. On day one of activation, request the Azure OpenAI model deployment, since
availability and quota vary by region and subscription.

| # | Build | Learn first | Done when |
|---|---|---|---|
| 0.1 | Public repo, solution skeleton `RoadSafe.slnx` following the dependency rule (one host plus one example module that shows the layering, with an automated dependency check; module boundaries are decided in 1.1, CR-001), `docs/adr/`, `infra/`, shared build settings, CI on GitHub Actions, README, and ADR-0001 written by me (why modular monolith + ML service + worker + Functions, not full microservices, and how the timer Function's job differs from the worker's, CR-003) | ADRs: [adr.github.io](https://adr.github.io). GitHub Actions quickstart (unverified). | CI is green on a PR |
| 0.2 | Hello API with health endpoint, multi-stage Dockerfile, non-root container | Microsoft Learn: containerize a .NET app (unverified). | `docker run` works; image is small and non-root |
| 0.3 | Bicep for resource group, Log Analytics, Application Insights, container registry, Container Apps environment, Key Vault and the app. Budget alert FIRST. | [Fundamentals of Bicep](https://learn.microsoft.com/training/paths/fundamentals-bicep/) (verified) | One command builds the environment; I reviewed `what-if` before deploying |
| 0.4 | GitHub Actions pipeline: build, test, push, deploy, authenticating with OIDC (no stored credential) | "Bicep with GitHub Actions" quickstart, linked from the [Bicep docs](https://learn.microsoft.com/azure/azure-resource-manager/bicep) (verified) | A push to main deploys |
| 0.5 | Key Vault, managed identity, least-privilege RBAC. App reads a secret via `DefaultAzureCredential`, same code runs locally | [Authenticating Azure-hosted apps with the Azure SDK for .NET](https://learn.microsoft.com/dotnet/azure/sdk/authentication/azure-hosted-apps) (verified) | No secret in env vars, config or git |
| 0.6 | Azure Function (timer trigger, isolated worker model) calls the API health endpoint; deployed through Bicep and the pipeline | [Quickstart: run scheduled tasks using Azure Functions](https://learn.microsoft.com/en-us/azure/azure-functions/scenario-scheduled-tasks) (verified). In-process model support ends 2026-11-10, so use isolated. | Runs on schedule in Azure |
| 0.7 | OpenTelemetry to Application Insights from the API and the Function, plus one custom span | [Enable Azure Monitor OpenTelemetry](https://learn.microsoft.com/en-us/azure/azure-monitor/app/opentelemetry-enable) (verified). Read its release-status note before choosing a package. | One request shows as a correlated trace |
| 0.8 | Entra ID app registrations; protect one endpoint with bearer tokens | [Quickstart: protect a web API with Microsoft.Identity.Web](https://learn.microsoft.com/entra/msidweb/getting-started/quickstart-webapi) (verified) | No token gives 401; a valid token gives 200 |
| 0.9 | One `IChatClient` call to Azure OpenAI using managed identity; swap to Ollama by configuration only | [Build an AI chat app with .NET](https://learn.microsoft.com/dotnet/ai/quickstarts/build-chat-app) (verified) | Same code, two providers |
| 0.10 | Tear everything down, redeploy from zero via the pipeline, check cost, write ADR-0002 and a README diagram | none | Repeatable rebuild; cost recorded |

## Phase 1: Foundation and Prediction Core
1.1 Domain model and Clean Architecture/CQRS skeleton, Postgres and EF Core. Module boundaries are decided here, with their own ADR (CR-001).
1.2 US Accidents ingestion and cleaning, H3 grid, features
1.3 Model training with experiment registry and SHAP
1.4 FastAPI model service and its .NET integration
1.5 Read API, Redis caching, analyst dashboard map
1.6 Test pyramid with Testcontainers

## Phase 2: AI Copilot
2.1 Embeddings, ingestion and chunking of the safety knowledge base
2.2 Hybrid search, reranking, source attribution
2.3 AI abstraction layer for provider swapping
2.4 Agent with tool-calling over the prediction engine
2.5 MCP server and human-in-the-loop approvals

## Phase 3: Trust, Quality and Observability
3.1 Guardrails, structured-output validation, PII protection, permission-aware retrieval
3.2 Red-team suite (promptfoo, garak, PyRIT) running in CI
3.3 Evals and CI gates, prompt versioning, prompt regression tests
3.4 Tracing across model, retrieval and tool calls, token and cost monitoring

## Phase 4: Production Hardening and Cloud
4.1 Event-driven core with outbox and the worker
4.2 Public portal and Entra ID roles
4.3 Full IaC, pipelines, secrets, and the final Azure burst within budget

## Phase 5: Stretch
Extract one real service (likely the copilot), driver-facing route check, alerting.

Phases 1 to 5 are outlines. The strategy thread writes each phase in detail when we
reach it, because details written early go stale.
