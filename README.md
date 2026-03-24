# debate-skill

Multi-perspective analysis powered by a team of independent agents. One is always Codex (different model family) for genuine diversity.

```
claude skill install hksw-io/debate-skill
```

## Quick start

```sh
/debate Should we migrate from REST to GraphQL?
/debate size:5 depth:deep Build vs buy for notifications
/debate Why is checkout dropping 30% at payment?
```

## Arguments

| Arg | Example | Description |
|-----|---------|-------------|
| `size:N` | `size:5` | Panel size (3-7) |
| `depth:X` | `depth:deep` | quick (1 round), standard (2-3), deep (4+) |
| *(rest)* | `Should we migrate?` | Topic (required) |

## What happens

1. **Context** — explores codebase, asks high-impact questions if needed
2. **Complexity** — scores topic (5-15), sets panel size and Codex reasoning effort
3. **Perspectives** — derives domain-specific roles from topic (e.g., "SwiftData Expert", "QA Lead", "Product Owner"), not generic archetypes
4. **Team** — spawns one agent per perspective, all working in parallel
5. **Rounds** — thesis → antithesis → synthesis, with full history passed each round
6. **Report** — consensus labels, decision, trade-offs, surprise finding, strongest dissent, evidence quality

## Design

- **Always a team** — independent agents, not one model playing all roles
- **Codex is a first-class member** — same task list, messaging, and coordination as Claude agents
- **Domain roles** — "Core Data Advocate" not "Contrarian"
- **Platform quality** — auto-detects Apple/Microsoft/Android/Web and adds a platform-native quality perspective
- **Cumulative context** — every round gets full debate history, critical for stateless Codex agent
- **Evidence tagging** — claims are tagged [grounded], [informed], or [speculative]

## Requirements

- [Codex CLI](https://github.com/openai/codex) installed
- `CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1` in Claude Code settings

## License

MIT
