# debate-skill

A Claude Code skill for structured multi-perspective analysis using Hegelian dialectic. Spawns a team of independent agents — each with a domain-specific role — to debate a topic. One perspective is always powered by Codex CLI for genuine model diversity.

## Install

```
claude skill install hksw-io/debate-skill
```

## Prerequisites

- Codex CLI installed (`/opt/homebrew/bin/codex`) for the Codex perspective
- `CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1` in Claude Code settings

## Usage

```
/debate Should we migrate from REST to GraphQL?
/debate size:5 depth:deep Build vs buy for our notification system
/debate Why is the checkout flow dropping 30% of users at payment?
```

## What it does

1. Scores topic complexity (5-15) to determine panel size and depth
2. Derives domain-specific perspective roles from the topic (not generic archetypes)
3. Spawns a team of independent agents — one per perspective, working in parallel
4. One perspective is always powered by Codex CLI (different model family)
5. Runs structured debate rounds: thesis, antithesis, synthesis
6. May ask clarifying questions if the debate needs context from you
7. Claude agents can also request ad-hoc Codex consultations mid-debate
8. Produces a final report with consensus labels, trade-offs, and actionable recommendations
9. Cleans up the team when done

## Key design decisions

- **Always spawns a team** — if you invoked `/debate`, you want a real debate with independent reasoning
- **Domain-specific roles** — "SwiftData Expert" and "QA Lead" instead of generic "Specialist" and "Skeptic"
- **Codex as first-class team member** — spawned as an Agent like everyone else, participates in task list and messaging
- **No fictional characters** — role labels only, direct analytical tone
- **Plain text output** — no box-drawing characters, no emoji

## License

MIT
