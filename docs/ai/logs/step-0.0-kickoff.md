# Step 0.0: Kickoff

- Date started / finished: 2026-11-02 / 2026-11-03
- Thread type: Claude Code, plan mode (writes in `docs/ai/` only after approval)
- Hours spent (rough): not recorded

## Outcome
The mentor setup was checked and hardened. The brief and roadmap were reviewed, and 18
change requests were logged. CR-001 to CR-003 are decided and recorded in the brief and
roadmap. The Step 0.1 prompt is ready.

## What I built
No code. My changes:
- `.claude/settings.json`: allow list narrowed to `docs/ai/**` edits plus `git status` and
  `git log`; the bare `Read` allow removed; Read denies for secrets outside the repo (home
  `.azure`, `.ssh`, `.kaggle`, the user-secrets folder) and for `*.pfx`, `*.pem`, `*.key`;
  `ask` rules for `git diff`, PROJECT-BRIEF.md and ROADMAP.md.
- MCP servers this repo does not need disabled through `/mcp`: a desktop file-and-shell
  server, plus the mail, storage, calendar, notes and design connectors.
- `.gitignore` added: the `dotnet new gitignore` template plus secret patterns (`.env*`,
  `!.env.example`, `local.settings.json`, `secrets.json`).

Claude Code wrote only in `docs/ai/`: CHANGE-REQUESTS.md, this log, START-HERE.md,
CONTENT-IDEAS.md, the LEARNING-PROMPT.md bracket count, and the CR-001 to CR-003 records in
PROJECT-BRIEF.md and ROADMAP.md.

## Acceptance criteria
| Criterion | Result | Evidence |
|---|---|---|
| Role, rules, change control and step routine restated | pass | kickoff reply, 2026-11-02 |
| Brief and roadmap reviewed; findings as CRs with a tier | pass | `CHANGE-REQUESTS.md`, CR-001 to CR-018 |
| Settings checked and `/permissions` run | pass | you ran `/permissions` and `/mcp`; settings as listed above |
| Both skills visible in the `/` menu | unclear | not confirmed in chat; check on the 0.1 machine |
| Up to 5 questions answered | pass | your answers, 2026-11-03 |
| Writes only inside `docs/ai/` | pass | file list above |

## Decisions and deviations
| Decision | Who decided | Why | Tier |
|---|---|---|---|
| 0.1 ships a minimal skeleton (one host plus one example module, with a dependency check); boundaries in 1.1; `RoadSafe.slnx` | strategy thread, 2026-11-03 | boundaries need the domain model first | 2, CR-001 |
| Open-decisions register in the brief; interim plain xUnit asserts | strategy thread, 2026-11-03 | no mid-step Tier 2 stops | 2, CR-002 |
| A timer Function ingests Open-Meteo weather; the worker consumes Service Bus events; ADR-0001 states the difference | strategy thread, 2026-11-03 | a real job for Functions | 2, CR-003 |
| CR-014 to CR-018 approved, applied at their steps | strategy thread, 2026-11-03 | small additions inside steps | 1 |
| CR-004 to CR-013 stay Proposed. CR-004 and CR-005 agreed in principle; CR-007 leaning to option (b), Aspire in 1.1 | strategy thread, 2026-11-03 | decided with the activation decision after 0.2, or when each phase is detailed | 2 |
| Every change from 0.1 goes through a PR with main protected; `/review-step` reviews the branch diff against main | you | small PRs; CI evidence on a PR | process |
| Step threads run in Manual mode | you | keeps the prompt as the guard for anything not allowed | process |
| CI evidence: you paste run links (gh not installed) | you | no extra tool needed | process |
| Setup gaps logged here, not as CRs | you | setup, not plan | n/a |

## Questions asked and answered
- Q: Will every change go through a PR, and how will CI evidence reach the review?
  A: Yes, main is protected from 0.1 on. gh is not installed, so you paste run links.
- Q: Toolchain? A: Visual Studio 2026 (July 2026 build), .NET SDK 10.0.303. Step 0.1 runs
  on a different Windows 11 laptop with 16 GB RAM and a 4 GB integrated GPU. Docker Desktop
  is not installed yet (needed from 0.2); WSL not checked yet.
- Q: Have you written GitHub Actions workflows or ADRs? A: No, so 0.1 starts with a
  learning thread.
- Q: Permission mode for step threads? A: Manual.
- Q: Decide CR-001 to CR-003 before 0.1, or start under interim rules? A: Decided in the
  strategy thread on 2026-11-03.

## Review findings
Brief and roadmap: 18 findings, logged as CR-001 to CR-018 in `CHANGE-REQUESTS.md`.

Setup gaps found on 2026-11-02, and what happened to them:
1. An MCP server with file-write and process tools sidestepped every Edit and Bash rule.
   Disabled via `/mcp`. Not denied in `settings.json`, so it is open again if re-enabled.
2. Connectors that can send or share were live. Disabled via `/mcp`.
3. Prefix-matched Bash denies are easy to sidestep (`git -C`, other git write commands,
   `Remove-Item`, `az rest`, Functions publish, `docker push`). Mitigated by Manual mode.
   Never use bypass or auto mode on this repo, and never click "don't ask again" on a
   command that can write.
4. `git diff` could print a denied file (`--no-index`) or write one (`--output=`) with no
   prompt. Now an `ask` rule.
5. Secrets outside the repo were readable through the bare `Read` allow. Bare allow
   removed; home-folder and key-file denies added.
6. BRIEF and ROADMAP were editable with no prompt. Now `ask` rules.
7. `.env.*` also hides `.env.example` from reviews (kept, your choice). There was no
   `.gitignore` (now added).

New on 2026-11-03:
8. **`.gitignore` line 36 (`[Ll]ogs/`) ignores `docs/ai/logs/`.** `git check-ignore`
   confirms that this log would never be committed. `_TEMPLATE.md` is tracked only because
   it was committed before the rule existed. Fix before committing.
9. `/permissions` first showed no allow rules from project settings. The rules you
   re-added through the popup landed in `.claude/settings.local.json`, which your global git
   ignore excludes, so they will not reach another machine. `settings.json` now lists them
   too: confirm on the 0.1 machine that they show with project settings as their source.
10. `/mcp` choices and local settings are per machine. Repeat the `/mcp` step on the 0.1
    machine.
11. `.gitignore` has no `*.pem`, `*.key` or `kaggle.json`. The Read denies protect against
    me, not against `git add`. Minor; worth adding by 1.2.

## Stretch challenges
None taken. Offered: a PreToolUse hook that rejects writes outside `docs/ai/`, which is
the version of the guard that cannot be talked around.

## Interview drill
None (kickoff).

## ADRs written
None. ADR-0001 is due in 0.1.

## Learning notes
0.1 needs a learning thread first. Filled LEARNING-PROMPT for the Claude Project:

```
I'm about to start RoadSafe step 0.1 Repo and engineering foundation. I'm new to GitHub
Actions (CI for a .NET 10 solution on pull requests, least-privilege permissions, pinning
actions to commit SHAs), GitHub rulesets, secret scanning and push protection, and
Architecture Decision Records.
The outcome I must reach: CI is green on a PR, with secret scanning and push protection
on, a ruleset on main that requires a PR and a passing CI check, a LICENSE and a
SECURITY.md, least-privilege workflow permissions, SHA-pinned actions, a test that checks
the dependency rule, and ADR-0001 merged.
Teach me only what I need for that outcome, in about 4 hours.

For each stage give: the idea in 2-3 lines, one hands-on exercise I can do outside the
project repo, and a "you're ready when" check. Name the single best free resource per
stage, and check the current docs before recommending a link. Do not write my project
code. Quiz me at the end with 5 questions, and tell me what to ask Claude Code to
confirm I'm ready.
```

## Cost impact
$0. Azure is not activated. Budget risks are logged as CR-005 and CR-006.

## Content idea
See `CONTENT-IDEAS.md`, "My AI coding agent's deny rules had gaps". Publish only after
every gap is closed, and without naming accounts.

## Open items and risks
- Finding 8: fix the `.gitignore` logs rule before committing `docs/ai/`.
- Commit the settings, `.gitignore` and `docs/ai/` changes before 0.1 starts on the other
  machine and before main is protected.
- Update the copy of the brief in the Claude Project (CR-001 to CR-003 changed it).
- 0.1 machine: confirm the git identity there is the one you want in public commit
  history; repeat `/permissions` and `/mcp`; confirm `/review-step` and `/finish-step` are
  listed.
- 0.2: Docker Desktop is not installed and WSL is not checked. Check Docker Desktop's
  licence terms and any policy for the device before installing.
- Later, when Azure work starts: make sure the Azure CLI on that machine signs in to the
  project's tenant and subscription, not another one.
- Local-first capacity: 16 GB RAM and an integrated GPU limit local models to small ones.
  GitHub Models for prototyping matters more. Input for CR-006 and Phase 2 detail.
- Open Tier 2: CR-004 to CR-013. Bring CR-004 to CR-007 to the activation decision after
  0.2.

## Prompt for the next step thread

```
RoadSafe, Step 0.1: Repo and engineering foundation.
Run the Mentor Mode session protocol.

Read docs/ai/START-HERE.md and docs/ai/logs/step-0.0-kickoff.md first. This thread runs
on a different machine from the kickoff.

Outcome: a public repo with a minimal .NET 10 solution skeleton, shared build settings,
CI on GitHub Actions running on pull requests, a truthful README and ADR-0001, with main
protected.

Build (ROADMAP 0.1, as amended by CR-001 and CR-003): public repo; solution skeleton
RoadSafe.slnx following the dependency rule (one host plus one example module that shows
the layering, with an automated dependency check; module boundaries are decided in 1.1);
docs/adr/; infra/; shared build settings; CI on GitHub Actions; README; ADR-0001 written
by me (why modular monolith + ML service + worker + Functions, not full microservices,
and how the timer Function's job differs from the worker's).

Acceptance criteria
- From ROADMAP: CI is green on a PR.
- Approved Tier 1 additions (CR-014; apply in this step, then mark it Applied):
  - Secret scanning and push protection are on.
  - A ruleset on main requires a PR and a passing CI check.
  - The repo has a LICENSE (my choice) and a SECURITY.md.
  - Workflows use least-privilege permissions and actions pinned to commit SHAs.
  - A test checks the dependency rule. If that needs a library, raise a Tier 2 item
    first (CR-002).
  - ADR-0001 is merged.

Decisions carried forward
- CR-001: RoadSafe.slnx; minimal skeleton; no module boundaries yet.
- CR-002: plain xUnit asserts. Do not add MediatR, MassTransit or FluentAssertions. Any
  new library is a Tier 2 item.
- CR-003: a timer Function ingests Open-Meteo weather; the worker consumes Service Bus
  events. ADR-0001 states the difference. No Function is built in this step.
- Every change goes through a PR; main is protected. /review-step reviews the branch diff
  against main.
- gh is not installed: I paste CI run links as evidence.
- Toolchain: Visual Studio 2026, .NET SDK 10.0.303, Windows 11, 16 GB RAM.
- Mode: Manual. CR-004 to CR-013 stay Proposed: do not act on them.

Setup check on this machine, before the brief
1. main contains the kickoff commits (settings.json, .gitignore, docs/ai).
2. /permissions matches .claude/settings.json, with project settings as the source.
   /mcp shows the servers disabled at the kickoff are disabled here too. The / menu lists
   /review-step and /finish-step.
3. docs/ai/logs/ is no longer ignored (kickoff log, finding 8).
4. My git identity on this machine is the one I want in public history.

Learn first: I have never written GitHub Actions workflows or ADRs. The filled
LEARNING-PROMPT is in the kickoff log under "Learning notes". Before the brief, confirm I
finished that learning thread and passed its readiness check; if I haven't, send me
there. Starting resources (ROADMAP): adr.github.io; the GitHub Actions quickstart (link
unverified).
```
