# Content Ideas (LinkedIn and YouTube)

Optional. Claude Code adds an entry only when a step has a strong teaching angle: a
surprising bug, a real trade-off, or a demo-worthy result. Most steps will add nothing.
I decide whether to make anything.

## Entry format
```
### Idea: short title
- Step:
- Format: LinkedIn post | short video | longer video
- Angle (what others learn):
- Audience (for example .NET developers new to AI):
- What to show or record (a local demo is fine):
- Safe to share? (no secrets, no account details, no employer information)
- Status: idea | drafted | published
```

## Ideas

### Idea: My AI coding agent's deny rules had gaps
- Step: 0.0 kickoff
- Format: LinkedIn post (a short video works too)
- Angle (what others learn): deny rules for a coding agent cover only some of its tools.
  MCP servers with file or shell tools are not covered by file and shell rules,
  prefix-matched shell denies are easy to sidestep, and a "read-only" allow like
  `git diff` can print a denied file. What actually protects you: a small allow list,
  prompts for everything else, `ask` rules for files that need a human, and disabling the
  servers a repo does not need. This is least-privilege tool calling applied to your own
  agent.
- Audience (for example .NET developers new to AI): developers adopting AI coding agents;
  .NET developers new to AI security.
- What to show or record (a local demo is fine): the settings before and after; one
  harmless probe (reading a dummy `.env` is denied); the list of gap types.
- Safe to share? (no secrets, no account details, no employer information): only after
  every gap is closed. Do not name connected accounts or services, and do not show paths
  containing the machine's user name.
- Status: idea
