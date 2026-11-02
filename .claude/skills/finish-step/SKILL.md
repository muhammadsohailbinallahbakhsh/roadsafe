---
name: finish-step
description: Closes a RoadSafe step after review. Pushes me past the acceptance criteria, runs an interview drill, checks ADR and pre-commit readiness, writes the step log, and produces the next-step prompt. Use only when I run /finish-step.
disable-model-invocation: true
---

# /finish-step: improve beyond the limits, then close the step

Optional argument: a step number such as `0.3`. Follow every hard rule in `CLAUDE.md`.
You write only in `docs/ai/`. I write all code, config and ADRs.

## 1. Gate
Run the `/review-step` procedure first. If any Blocker or Major remains, stop and say
so. Do not continue until they are fixed.

## 2. Think beyond the limits
Pick 3 stretch challenges specific to this step, each from a different lens: security,
failure modes, scale, cost, observability, testability, portability, developer
experience, maintainability. State each as a question or a scenario, never a solution.
Example shape: "What happens to this pipeline if the registry is down for ten minutes?"

Ask me to choose one or more. For each one I choose, make me propose my approach first.
Only after my attempt, review it. Ask my permission before suggesting concrete
improvements, and even then explain the idea rather than patching it. When I make
changes, review again.

## 3. Interview drill
Ask 3 questions an interviewer would ask about this step, mixing a how, a why-not-X
and a failure scenario. I answer first. Then critique my answers and add what a
staff-level answer would include.

## 4. ADR check
Say whether this step deserves an ADR (a real choice between options with a downside).
If I wrote one in `docs/adr/`, review it for context, options considered, decision, and
consequences including the downside I accepted.

## 5. Pre-commit check (read-only)
- Look for secrets in the changed files: keys, tokens, connection strings, passwords,
  `.env` files, `local.settings.json`, private keys. Report findings without echoing
  secret values.
- Look for generated or junk files that should not be committed, and sanity-check
  `.gitignore`.
- Ask before running build or tests. If I agree, run them and report.
- Confirm README and docs match reality.
- Suggest how to split the work into commits, and give conventional-commit messages as
  plain text. I commit through Visual Studio. Never run git write commands.

## 6. Close the step
1. Draft the "Decisions and deviations" section and show it to me. Ask me to confirm it
   is accurate before saving.
2. Write `docs/ai/logs/step-X.Y.md` from `docs/ai/logs/_TEMPLATE.md`.
3. Update the status table and "latest log" link in `docs/ai/START-HERE.md`.
4. Add every Tier 1 or Tier 2 item to `docs/ai/CHANGE-REQUESTS.md`.
5. Add to `docs/ai/CONTENT-IDEAS.md` only if there is a strong teaching angle.
6. Write the next-step prompt, self-contained: step id and name, outcome, acceptance
   criteria copied from ROADMAP, decisions carried forward, what to learn first, and the
   line "Run the Mentor Mode session protocol". Save it at the end of the log and show it
   to me in chat.
7. If any Tier 2 item is open, say clearly: "Bring this to your strategy thread before
   starting the next step."
