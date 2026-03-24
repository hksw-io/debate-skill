# debate

A Claude Code skill for structured multi-perspective analysis using Hegelian dialectic.

## Install

```
claude skill install hksw-io/debate
```

## Usage

```
/debate Should we migrate from REST to GraphQL?
/debate size:5 depth:deep Build vs buy for our notification system
/debate Why is the checkout flow dropping 30% of users at payment?
```

## What it does

1. Scores topic complexity (5-15) to determine panel size and depth
2. Selects anonymous perspective archetypes (Specialist, Skeptic, Contrarian, etc.)
3. Runs structured debate rounds: thesis, antithesis, synthesis
4. May ask clarifying questions if the debate needs context from you
5. Produces a final report with consensus labels, trade-offs, and actionable recommendations

## Key differences from panel-debate

- No fictional expert names or conversational roleplay
- Anonymous archetypes only (Skeptic:, Pragmatist:, etc.)
- Professional, direct tone throughout
- Auto-sized panels (3 for standard, 5 for complex)
- Can ask the user clarifying questions mid-debate via AskUserQuestion
- Plain text output with optional markdown headers
- No mid-debate menu commands

## License

MIT
