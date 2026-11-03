# Change Requests

Anything that changes, extends or suggests changing the plan is logged here AND said in
chat. I bring open items to my strategy thread, we decide together, and I paste the
decision back here.

## Tiers
- **Tier 1, minor:** applied inside the step and reported (sub-steps, learning topics, extra
  acceptance criteria, "(extension)" detail in ROADMAP). Status: `Applied`.
- **Tier 2, needs a decision:** stack, libraries, architecture, locked decisions, phases or
  steps, security, cost, dataset. Status: `Proposed`, then `Decided`. Work in the affected
  area stops until a decision is pasted here.

## Entry format
```
### CR-001: short title
- Date / step:
- Tier: 1 or 2
- Proposed by: Claude Code or me
- What changes:
- Why:
- Impact on phases/steps/cost/risk:
- Options considered:
- Status: Proposed | Applied | Decided | Rejected
- Decision (pasted from strategy thread):
```

## Log

Kickoff batch: Claude Code raised CR-001 to CR-018 in the step 0.0 kickoff on 2026-11-02
(see `logs/step-0.0-kickoff.md`). Decisions were pasted from the strategy thread on
2026-11-03.

| CR | Title | Tier | Status | Decide or apply at |
|---|---|---|---|---|
| CR-001 | What 0.1's skeleton contains | 2 | Decided | recorded in BRIEF and ROADMAP |
| CR-002 | Open-decisions register | 2 | Decided | recorded in BRIEF |
| CR-003 | A real job for Azure Functions | 2 | Decided | recorded in BRIEF and ROADMAP |
| CR-004 | Identity, RBAC and a bootstrap layer before the first deploy | 2 | Proposed | activation decision, after 0.2 |
| CR-005 | Phase 0 under the 30-day clock | 2 | Proposed | activation decision, after 0.2 |
| CR-006 | Budget after the credit ends | 2 | Proposed | activation decision, after 0.2 |
| CR-007 | When Aspire arrives, and what it owns | 2 | Proposed | activation decision, after 0.2 |
| CR-008 | Entra ID from Phase 1, drop ASP.NET Core Identity | 2 | Proposed | Phase 1 detail |
| CR-009 | Events and integration tests from Phase 1 | 2 | Proposed | Phase 1 detail |
| CR-010 | Prediction target and how a model wins | 2 | Proposed | Phase 1 detail |
| CR-011 | Evals, tracing and a threat model with Phase 2 | 2 | Proposed | Phase 2 detail |
| CR-012 | RAG corpus and its licences | 2 | Proposed | Phase 2 detail |
| CR-013 | Evidence for scale thinking | 2 | Proposed | Phase 4 detail |
| CR-014 | 0.1 acceptance for a public repo | 1 | Decided, apply at 0.1 | 0.1 |
| CR-015 | Split 0.3 into 0.3a and 0.3b | 1 | Decided, apply at 0.3 | 0.3 |
| CR-016 | 0.4 deploy safety | 1 | Decided, apply at 0.4 | 0.4 |
| CR-017 | 0.8 rejects wrong tokens | 1 | Decided, apply at 0.8 | 0.8 |
| CR-018 | 0.10 rebuild survives soft-delete | 1 | Decided, apply at 0.10 | 0.10 |

### CR-001: What 0.1's skeleton contains
- Date / step: 2026-11-02 / 0.0 kickoff
- Tier: 2 (architecture; locked decision wording)
- Proposed by: Claude Code
- What changes: Say what 0.1's "solution skeleton" contains versus 1.1's "Clean
  Architecture/CQRS skeleton", and when module boundaries are decided. Allow `.slnx` where
  the brief said `RoadSafe.sln`.
- Why: The brief locks a modular monolith but named no modules and no layering style
  (layers inside each module, or shared layers). 0.1 would have guessed and 1.1 would have
  redone it. Current .NET tooling produces `.slnx`.
- Impact on phases/steps/cost/risk: 0.1 scope and ADR-0001; 1.1 gains a boundaries decision
  and its own ADR. No cost.
- Options considered: (a) decide the module map now; (b) 0.1 ships one host plus one
  example module that shows the layering, with an automated dependency check, and 1.1
  decides the boundaries with its own ADR. Claude Code recommended (b): boundaries drawn
  before the domain is modeled are guesses.
- Status: Decided
- Decision (pasted from strategy thread): 2026-11-03: option (b). Use `.slnx`; the brief
  changes `RoadSafe.sln` to `RoadSafe.slnx`. Recorded in PROJECT-BRIEF.md (Backend, Repo,
  Repo layout) and ROADMAP.md (0.1, 1.1) on 2026-11-03.

### CR-002: Open-decisions register with decide-by steps
- Date / step: 2026-11-02 / 0.0 kickoff
- Tier: 2 (libraries; locked decisions)
- Proposed by: Claude Code
- What changes: Move the undecided items out of the "Locked" table into an "Open
  decisions" list, each with a decide-by step: assertion library (before 0.1's first test;
  interim plain xUnit asserts), MediatR (1.1), MassTransit (before outbox work, see
  CR-009), Semantic Kernel or Agent Framework (2.3), LightGBM or XGBoost (1.3, by
  experiment), Python and frontend toolchains (1.2, 1.5), map tile provider (1.5), frontend
  hosting (4.3), and the Azure AI Search swap, which no step schedules.
- Why: The "Locked" table contained two "or"s and the licensing item had no deadline. With
  the Tier 2 stop rule, that means work stops in the middle of a step.
- Impact on phases/steps/cost/risk: None on cost. Prevents mid-step stops.
- Options considered: a register in the brief (recommended), or tracking here only.
- Status: Decided
- Decision (pasted from strategy thread): 2026-11-03: decided as proposed. Interim: plain
  xUnit asserts. Recorded in PROJECT-BRIEF.md ("Open decisions", ML and AI layer rows) on
  2026-11-03.

### CR-003: Give Azure Functions a real job in the product
- Date / step: 2026-11-02 / 0.0 kickoff
- Tier: 2 (architecture)
- Proposed by: Claude Code
- What changes: Give the Function app a product responsibility after 0.6, and write down
  how its job differs from the worker's.
- Why: Functions are "required, not optional", but no step after 0.6 used them. A reviewer
  will ask why both a worker and Functions exist, and "to learn Functions" is not an
  architecture answer.
- Impact on phases/steps/cost/risk: ADR-0001 (0.1), then the detail of Phases 1, 2 and 4.
- Options considered: timer-driven Open-Meteo weather ingestion (recommended: naturally
  scheduled, idempotent, fits the free grant); scheduled retrain-and-promote;
  knowledge-base ingestion when a document lands in storage; threshold alerts (stretch).
- Status: Decided
- Decision (pasted from strategy thread): 2026-11-03: a timer Function ingests Open-Meteo
  weather; the worker consumes Service Bus events. ADR-0001 states the difference.
  Recorded in PROJECT-BRIEF.md (Other deployables) and ROADMAP.md (0.1) on 2026-11-03.

### CR-004: Identity, RBAC and a bootstrap layer before the first deploy
- Date / step: 2026-11-02 / 0.0 kickoff
- Tier: 2 (security posture; step order)
- Proposed by: Claude Code
- What changes: Teach managed identity and role assignments before or inside 0.3 (today
  they belong to 0.5, and app registrations to 0.8). Add a bootstrap layer that I deploy
  once with my own login: the pipeline's identity and federated credential, its role
  assignments, and the Entra app registrations. The pipeline itself gets resource-group
  rights only. 0.10's "from zero" then means everything except bootstrap.
- Why: In 0.3 the app must pull from the registry, which needs an identity with a pull
  role, or else the registry admin password (a stored secret). 0.4's OIDC login needs a
  federated credential and role assignments. A pipeline cannot create its own identity,
  and creating role assignments needs Owner-level rights.
- Impact on phases/steps/cost/risk: 0.3, 0.4, 0.5, 0.8, 0.10. Worth an ADR. No cost.
- Options considered: (a) bootstrap layer plus a pipeline limited to the resource group
  (recommended); (b) the pipeline gets the RBAC Administrator role with a condition
  limiting which roles it can assign (tighter least privilege, more to learn); (c) registry
  admin user or an Owner pipeline (reject).
- Status: Proposed
- Decision (pasted from strategy thread): Pending. Decided with the activation decision
  after 0.2. 2026-11-03: the strategy thread agrees as proposed; the formal decision is
  recorded then. Do not act on it before.

### CR-005: Phase 0 under the 30-day clock
- Date / step: 2026-11-02 / 0.0 kickoff
- Tier: 2 (step order and scope; budget)
- Proposed by: Claude Code
- What changes: (1) A preparation sub-phase before activation: the learning threads for
  0.3 to 0.9; the Bicep and the workflow written and checked offline (build and lint); one
  region chosen that offers every Phase 0 service; a check that a free account can get
  Azure OpenAI quota and supports budgets. (2) A hello FastAPI container added to Phase 0,
  deployed as an internal Container App that the API calls.
- Why: At 10-15 hours a week, 30 days is about 43-64 hours, roughly what eight
  new-technology steps need, with no slack. Without the hello service, the first cloud
  deploy of Python, and of one service calling another, happens in the final burst, on
  real money.
- Impact on phases/steps/cost/risk: Phase 0 order. The hello service adds 3-5 hours at
  near-zero cost.
- Options considered: (1) the prep sub-phase (recommended) or keep the order and accept
  the risk; (2) the hello service (recommended if (1) is accepted) or a rehearsal deploy
  at the start of 4.3.
- Status: Proposed
- Decision (pasted from strategy thread): Pending. Decided with the activation decision
  after 0.2. 2026-11-03: the strategy thread agrees as proposed; the formal decision is
  recorded then. Do not act on it before.

### CR-006: Budget after the credit ends
- Date / step: 2026-11-02 / 0.0 kickoff
- Tier: 2 (cost)
- Proposed by: Claude Code
- What changes: Write down how 4.3, and any Azure use after day 30, is paid for.
  Continuing means pay-as-you-go: no spending limit, and budgets only alert. Add a
  per-service estimate for the final burst (Postgres with PostGIS and pgvector, Redis, a
  Service Bus tier that supports topics, the registry, model tokens). Decide which model
  the CI evals and red-team call: GitHub Models through the workflow token, or Ollama on
  the runner, never a paid endpoint on every PR.
- Why: The $20-50 cap is enforced only by alerts and teardown. Some of these services bill
  by the hour from creation, and a CI gate calling a paid model spends money unnoticed.
- Impact on phases/steps/cost/risk: Phase 3 CI design; the shape of 4.3.
- Options considered: a cost table in the brief, updated with real numbers at 0.10
  (recommended).
- Status: Proposed
- Decision (pasted from strategy thread): Pending. Decided with the activation decision
  after 0.2.

### CR-007: When Aspire arrives, and what it owns
- Date / step: 2026-11-02 / 0.0 kickoff
- Tier: 2 (step scope)
- Proposed by: Claude Code
- What changes: Name the step that introduces Aspire. Record that Aspire handles local
  orchestration only, and that Azure IaC stays hand-written Bicep rather than generated by
  Aspire or azd.
- Why: Aspire is locked but no step introduces it. It can also generate Azure IaC and set
  up OpenTelemetry, which overlaps 0.3 and 0.7: two sources of truth otherwise.
- Impact on phases/steps/cost/risk: 0.2 or 1.1 scope; 0.7 gets smaller if Aspire comes
  first.
- Options considered: (a) 0.2, local and free, so traces show in the Aspire dashboard
  before 0.7 (Claude Code's recommendation; trade-off: the template sets up OpenTelemetry
  for you, so 0.7 teaches less by hand); (b) 1.1, alongside Postgres and Redis.
- Status: Proposed
- Decision (pasted from strategy thread): Pending. Decided with the activation decision
  after 0.2. 2026-11-03: the strategy thread agrees with option (b), Aspire arrives in
  1.1, not 0.2; the formal decision is recorded then. Do not act on it before.

### CR-008: Entra ID from Phase 1, drop ASP.NET Core Identity
- Date / step: 2026-11-02 / 0.0 kickoff
- Tier: 2 (locked decision: Auth)
- Proposed by: Claude Code
- What changes: The analyst/admin portal uses Entra ID from Phase 1 (a tenant and app
  registrations exist from 0.8 on, the free tier costs nothing, and it works in local
  dev). The public portal stays anonymous. Authorization is written against claims, so the
  identity provider can be swapped.
- Why: Approvals (2.5) and permission-aware retrieval (3.1) built on Identity roles would
  be rewired in 4.2. Identity adds password storage to defend and teaches little new.
- Impact on phases/steps/cost/risk: Removes the 4.2 migration; Phase 1 and 2 detail.
- Options considered: (a) Entra from Phase 1 (recommended); (b) keep the plan, with
  authorization behind an abstraction; (c) `dotnet user-jwts` tokens in dev, Entra in
  Azure.
- Status: Proposed
- Decision (pasted from strategy thread): Pending. Decided when Phase 1 is detailed.

### CR-009: Event-driven core and integration tests from Phase 1
- Date / step: 2026-11-02 / 0.0 kickoff
- Tier: 2 (phase order and scope)
- Proposed by: Claude Code
- What changes: Domain events and an outbox table from 1.1, dispatched in-process at first;
  the Service Bus transport and the worker come in at 4.1. Testcontainers integration tests
  from 1.1; 1.6 becomes contract tests (.NET to FastAPI) and end-to-end tests.
- Why: Retrofitting the event-driven core in Phase 4 touches every handler built in Phases
  1 to 3. A separate test step after the features contradicts "tests for behavior" in
  every step.
- Impact on phases/steps/cost/risk: Phase 1 and 4 detail; brings the MassTransit decision
  forward (CR-002).
- Options considered: as above, or keep the current order and accept the rework.
- Status: Proposed
- Decision (pasted from strategy thread): Pending. Decided when Phase 1 is detailed.

### CR-010: Define the prediction target and how a model wins
- Date / step: 2026-11-02 / 0.0 kickoff
- Tier: 2 (dataset; ML design)
- Proposed by: Claude Code
- What changes: Decide the label (for example "accident in this cell in this hour"),
  negative sampling, historical weather for hours without accidents, how traffic exposure
  is handled (or state that scores mean frequency, not rate), time-based and spatial splits
  against leakage, the metric behind "beats the current model", calibration, and a model
  card with limitations.
- Why: US Accidents holds accidents only: no non-accident records, weather only at
  accident time, no traffic volume. Its "Severity" means traffic delay, not injury, and the
  data ends in 2023. Without these decisions "risk" has no definition. It may need a second
  data source, which is a dataset decision.
- Impact on phases/steps/cost/risk: 1.2 and 1.3; possibly a new dataset. The licence check
  belongs before 1.2.
- Options considered: to be worked out when Phase 1 is detailed.
- Status: Proposed
- Decision (pasted from strategy thread): Pending. Decided when Phase 1 is detailed.

### CR-011: Evals, tracing and a threat model with Phase 2, not after it
- Date / step: 2026-11-02 / 0.0 kickoff
- Tier: 2 (phase order; security)
- Proposed by: Claude Code
- What changes: A golden question set and retrieval metrics from 2.1-2.2 (the CI gates stay
  in 3.3); GenAI tracing from 2.3; a threat-model step before 2.1 (STRIDE plus the OWASP
  Top 10 for LLM applications) that drives 3.1 and 3.2.
- Why: Tuning chunking, hybrid search and reranking without measurements is guesswork.
  Without a threat model, red-teaming tests whatever comes to mind.
- Impact on phases/steps/cost/risk: One added step; Phase 2 and 3 detail.
- Options considered: as above, or keep evals and tracing in Phase 3.
- Status: Proposed
- Decision (pasted from strategy thread): Pending. Decided when Phase 2 is detailed.

### CR-012: Name the RAG corpus and its licences
- Date / step: 2026-11-02 / 0.0 kickoff
- Tier: 2 (dataset)
- Proposed by: Claude Code
- What changes: List the knowledge-base sources with each one's licence. Prefer
  public-domain US federal material (NHTSA, FHWA, CDC). Add a few synthetic internal
  documents so permission-aware retrieval has something to protect. Commit no third-party
  documents whose licence does not allow it.
- Why: "Road-safety material" is undefined, some well-known manuals are copyrighted, and
  the repo is public.
- Impact on phases/steps/cost/risk: 2.1 and 3.1.
- Options considered: to be worked out when Phase 2 is detailed.
- Status: Proposed
- Decision (pasted from strategy thread): Pending. Decided when Phase 2 is detailed.

### CR-013: Evidence for "scale thinking"
- Date / step: 2026-11-02 / 0.0 kickoff
- Tier: 2 (adds a step; adds a tool)
- Proposed by: Claude Code
- What changes: SLOs for the read API and a load test, with the results in the README,
  before the final burst.
- Why: Scale thinking is a stated goal and no step produces evidence of it.
- Impact on phases/steps/cost/risk: One added step and a load-testing tool. Runs locally,
  so no Azure cost unless run against the final burst.
- Options considered: to be worked out when Phase 4 is detailed.
- Status: Proposed
- Decision (pasted from strategy thread): Pending. Decided when Phase 4 is detailed.

### CR-014: 0.1 acceptance for a public repo
- Date / step: 2026-11-02 / 0.0 kickoff
- Tier: 1 (added acceptance criteria)
- Proposed by: Claude Code
- What changes: Add to 0.1's acceptance criteria: secret scanning and push protection are
  on; a ruleset on main requires a PR and a passing CI check; the repo has a LICENSE (my
  choice) and a SECURITY.md; workflows use least-privilege `permissions` and actions pinned
  to commit SHAs; a test checks the dependency rule (a library for it is a Tier 2 item
  first, see CR-002); ADR-0001 is merged.
- Why: "CI is green on a PR" passes even with an empty workflow, and a secret pushed to a
  public repo stays public for good.
- Impact on phases/steps/cost/risk: 1-2 extra hours in 0.1. Free for public repos.
- Options considered: Dependabot now or at Phase 1 (left to the step).
- Status: Decided (approved as Tier 1 on 2026-11-03); apply at 0.1, then mark Applied.
- Decision (pasted from strategy thread): 2026-11-03: approved as Tier 1, apply at its step.

### CR-015: Split 0.3 into 0.3a and 0.3b
- Date / step: 2026-11-02 / 0.0 kickoff
- Tier: 1 (sub-steps)
- Proposed by: Claude Code
- What changes: 0.3a: budget alert, resource group, Log Analytics, Application Insights.
  0.3b: registry, Container Apps environment, Key Vault, the app. Each gets its own
  `what-if` review.
- Why: Seven resources, two deployment scopes, and a first IaC experience in a single
  review is too much.
- Impact on phases/steps/cost/risk: Two status rows instead of one. No cost change.
- Options considered: two or three sub-steps (left to the step).
- Status: Decided (approved as Tier 1 on 2026-11-03); apply at 0.3, then mark Applied.
- Decision (pasted from strategy thread): 2026-11-03: approved as Tier 1, apply at its step.

### CR-016: 0.4 deploy safety
- Date / step: 2026-11-02 / 0.0 kickoff
- Tier: 1 (added acceptance criteria)
- Proposed by: Claude Code
- What changes: The deploy job runs in a GitHub environment that needs my approval; the
  federated credential trusts only that environment; PR and fork runs get no Azure access;
  CI stays green when the Azure environment does not exist.
- Why: After 0.10's teardown I work locally for months, and "a push to main deploys" would
  then fail on every push. Anyone can open a PR on a public repo.
- Impact on phases/steps/cost/risk: 0.4 detail. Free.
- Options considered: none beyond the above.
- Status: Decided (approved as Tier 1 on 2026-11-03); apply at 0.4, then mark Applied.
- Decision (pasted from strategy thread): 2026-11-03: approved as Tier 1, apply at its step.

### CR-017: 0.8 rejects wrong tokens
- Date / step: 2026-11-02 / 0.0 kickoff
- Tier: 1 (added acceptance criterion)
- Proposed by: Claude Code
- What changes: A token issued for a different audience, or one missing the required scope
  or role, is rejected.
- Why: "No token gives 401" is the easy half. The real bugs hide in tokens that look valid.
- Impact on phases/steps/cost/risk: 0.8 tests. Free.
- Options considered: none beyond the above.
- Status: Decided (approved as Tier 1 on 2026-11-03); apply at 0.8, then mark Applied.
- Decision (pasted from strategy thread): 2026-11-03: approved as Tier 1, apply at its step.

### CR-018: 0.10 rebuild survives soft-delete
- Date / step: 2026-11-02 / 0.0 kickoff
- Tier: 1 (added acceptance criterion)
- Proposed by: Claude Code
- What changes: A rebuild from zero succeeds on the same day as the teardown; resource
  naming and the teardown handle soft-deleted Key Vault and Azure OpenAI accounts; purge
  protection is a deliberate choice.
- Why: Those resources keep their names, and the model quota, for a while after deletion,
  so a "repeatable rebuild" often fails on its second run.
- Impact on phases/steps/cost/risk: Naming parameters in 0.3; 0.10 teardown. Free.
- Options considered: none beyond the above.
- Status: Decided (approved as Tier 1 on 2026-11-03); apply at 0.10, then mark Applied.
- Decision (pasted from strategy thread): 2026-11-03: approved as Tier 1, apply at its step.
