# START HERE

Every Claude Code session on this repo reads this file first. Claude Code may update it
(status, links). Keep it short and accurate.

## Read in this order
1. `CLAUDE.md`: your role and the rules (loaded automatically)
2. `docs/ai/PROJECT-BRIEF.md`: what we are building and what is locked
3. `docs/ai/ROADMAP.md`: the steps, acceptance criteria and learn-first links
4. `docs/ai/CHANGE-REQUESTS.md`: open proposals and decisions
5. The latest step log (see below) for what just happened

## Where things are
- Step logs: `docs/ai/logs/` (template: `_TEMPLATE.md`)
- Content ideas: `docs/ai/CONTENT-IDEAS.md`
- Learning prompt template: `docs/ai/LEARNING-PROMPT.md`
- ADRs (written by me): `docs/adr/`
- Skills: `/review-step` (review my work), `/finish-step` (close the step)

## How a step thread runs
Brief, then I build, then hints only when I am stuck, then `/review-step`, then
`/finish-step`, which writes the log and gives me the prompt for the next thread.

Working rules not covered in CLAUDE.md:
- I start `/review-step` and `/finish-step`. Claude Code cannot, because both skills set
  `disable-model-invocation`.
- From 0.1 on, every change goes through a PR and main is protected. `/review-step` reviews
  the branch diff against main.
- CI evidence: I paste the run link (gh is not installed).
- Step threads run in Manual mode.

## Status
| Step | State | Log |
|---|---|---|
| 0.0 Kickoff (plan mode) | done | [step-0.0-kickoff](logs/step-0.0-kickoff.md) |
| 0.1 Repo and engineering foundation | not started (learning thread first) | |
| 0.2 Hello API and container | not started | |
| 0.3 to 0.10 | not started (0.3 splits into 0.3a and 0.3b, CR-015) | |

States: not started, in progress, in review, done, blocked.

## Current
- Current step: 0.1. Its prompt is at the end of the latest log.
- Next action (me): fix the `.gitignore` rule that ignores `docs/ai/logs/` (kickoff log,
  finding 8), commit the kickoff changes, then run the 0.1 learning thread.
- Latest log: [step-0.0-kickoff](logs/step-0.0-kickoff.md)
- Tier 2 items blocking 0.1: none (CR-001 to CR-003 decided 2026-11-03)
- Open Tier 2 change requests: CR-004 to CR-013. CR-004 to CR-007 are decided with the
  activation decision after 0.2; CR-008 to CR-013 when each phase is detailed.
- Approved Tier 1, applied at their steps: CR-014 (0.1), CR-015 (0.3), CR-016 (0.4),
  CR-017 (0.8), CR-018 (0.10)
- Setup to-dos and machine notes: kickoff log, "Open items and risks"
- Azure subscription: NOT activated (decided in the strategy thread)
