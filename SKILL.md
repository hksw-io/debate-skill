---
name: debate
description: >
  Structured multi-perspective analysis using Hegelian dialectic. Spawns a team
  of independent agents — each representing a domain-specific perspective — to
  debate a topic across multiple rounds. One perspective is always powered by
  Codex CLI for genuine model diversity. Produces synthesized conclusions with
  consensus labels and actionable recommendations.
license: MIT
metadata:
  version: "2.0"
---

# Structured Debate

Spawn a team of independent agents to analyze a topic from multiple perspectives using Hegelian dialectic.

## Arguments

Parse `$ARGUMENTS`:
- `size:N` -> panel size override (3-7)
- `depth:X` -> quick (1 round), standard (2-3), deep (4+)
- Remaining text -> topic

## Flow

```
COMPLEXITY_CHECK -> PERSPECTIVE_SELECTION -> TEAM_SETUP -> PARALLEL_ROUNDS -> SYNTHESIS -> REPORT -> CLEANUP
```

## Complexity Check

Score the topic on 5 dimensions (1-3 each):

| Dimension | 1 (Low) | 2 (Medium) | 3 (High) |
|-----------|---------|------------|----------|
| Stakeholders | Single group | 2-3 groups | 4+ groups |
| Trade-offs | Clear winner | 1-2 trade-offs | 3+ trade-offs |
| Time horizon | Immediate only | Months | Years |
| Reversibility | Easily reversed | Partially | Irreversible |
| Domain breadth | Single domain | 2 domains | 3+ domains |

Total = sum (range 5-15).

| Score | Action |
|-------|--------|
| 5-7 | Warn: topic may be simple. If user proceeds: 3 perspectives, 1-2 rounds |
| 8-11 | Standard: 3-5 perspectives, 2 rounds |
| 12-15 | Deep: 5-7 perspectives, 3+ rounds. Load all reference files. |

Override panel size with `size:N`.

## Perspective Selection

Read [references/perspectives.md](references/perspectives.md) for detailed guidance.

Derive **domain-specific perspectives from the topic context**, not generic archetypes. Analyze the topic for: technologies mentioned, stakeholder groups, domain areas, and constraint dimensions. Generate role labels that a real team would have for this specific problem.

**Two structural roles** (always present):
1. **Contrarian** — challenges consensus and surfaces alternatives. Frame in domain terms (e.g., for a migration topic: "Core Data Advocate" who argues against migration).
2. **Synthesizer** — connects viewpoints and finds integration points. Also domain-specific (e.g., "iOS Platform Engineer" who sees the full system picture).

**Remaining roles** (1-5 additional) are derived from the topic:
- What technologies or systems are involved? -> specialists for each
- Who are the stakeholders? -> representatives for key groups (product owner, QA lead, etc.)
- What constraints matter? -> perspectives focused on those (security, performance, cost, etc.)

**One perspective is always powered by Codex CLI.** The team lead assigns it to whichever role fits best. This provides genuine model diversity — a different model family reasoning about the same problem.

No fictional names. No fictional backgrounds. No personality traits. Label by role only.

## Clarifying Questions

Before or between rounds, if the debate needs information to give a well-grounded analysis, use the AskUserQuestion tool.

Guidelines:
- Ask when a key assumption would change the direction of the debate (e.g., team size, budget, timeline, existing constraints).
- Batch related questions into a single AskUserQuestion call (up to 4 questions).
- Phrase questions from the debate's perspective: "The debate needs to know your current deployment frequency to assess migration risk."
- Propagate answers to all perspectives via SendMessage after receiving clarification.
- Prefer reading codebase files over asking. Do not over-ask.

## Codex Consultation

During Round 2+, any Claude perspective agent can request an ad-hoc Codex consultation for a specific factual or technical question. This is separate from the mandatory Codex perspective — it's for when an agent wants to verify a specific claim.

- Max 2 ad-hoc consultations per debate.
- The requesting agent must state what it wants to learn and why.
- Invoke via Bash: `/opt/homebrew/bin/codex exec -s read-only --full-auto "PROMPT"`
- Results are shared with all perspectives.

## Team Execution

Every debate spawns a team. All perspectives — including the Codex-powered one — are first-class team members.

### Step 1: Create team

Use TeamCreate with name `debate-{topic-slug}`.

### Step 2: Create tasks

Use TaskCreate to create round tasks:
- `round-1-opening`: each perspective states opening position
- `round-2-response`: each perspective responds to others
- Additional round tasks as needed based on depth

### Step 3: Spawn perspective agents

Spawn one Agent per perspective role:
- `team_name`: the debate team name
- `subagent_type`: `"general-purpose"` (needs Bash for Codex, Read/Grep for codebase)
- `name`: slug of the role (e.g., `swiftdata-expert`, `product-owner`, `contrarian`)

**Claude perspective agent prompt template:**

> You are the [ROLE] in a structured debate about: [TOPIC]
>
> Your expertise: [DOMAIN DESCRIPTION]
>
> Round [N] instructions: [STATE OPENING POSITION / RESPOND TO OTHERS]
>
> [For Round 2+: Here are all perspectives from the previous round: ...]
>
> Rules:
> - Be direct and analytical. No conversational filler.
> - Label your output with your role name.
> - You may read codebase files if relevant to grounding your analysis.
> - Check TaskList for your assigned task and mark it complete when done.

**Codex perspective agent prompt template:**

Same role/topic/round information as any other agent, plus this instruction:

> You do not reason about this topic yourself. Instead, for each round:
> 1. Take your role description, the topic, and any prior round statements
> 2. Construct a prompt that captures all of this context
> 3. Run it via Bash: `/opt/homebrew/bin/codex exec -s read-only --full-auto 'PROMPT'`
> 4. Return the Codex output as your position statement
>
> You are a relay between the debate and Codex CLI. Participate in the task list and messaging like any other agent.

### Step 4: Execute rounds

All agents are treated identically by the team lead:

**Round 1 (Thesis):** All agents work their opening position task in parallel. Team lead collects all outputs.

**Round 2+ (Antithesis):** Team lead sends all positions to each agent via SendMessage, asks them to respond. All agents work in parallel. Before synthesis, the Contrarian is asked: "What are we missing?"

**Per-round synthesis (by team lead):**

After collecting all round outputs, the team lead produces:

- Agreements: where perspectives converge, noting same vs different reasoning
- Tensions: specific disagreements categorized as factual, values-based, methodological, or scope
- Open questions: what remains unresolved and what would resolve it
- Emerging insights: novel understanding not present in any single perspective
- Codex consultations: if used this round, what was asked and learned

Convergence rule: end early if all agree or cycling. Extend if major tension unexplored.

Validation: no "it depends" without concrete factors and decision criteria.

### Step 5: Cleanup

Send shutdown messages to all agents via SendMessage, then TeamDelete.

## Synthesis Rules

Read [references/synthesis-patterns.md](references/synthesis-patterns.md) for detailed patterns.

Consensus labels:
- UNANIMOUS: All perspectives agree on conclusion and reasoning
- STRONG: All agree on conclusion via different reasoning paths
- MAJORITY: Most agree, minority view acknowledged
- CONTESTED: Fundamental disagreement remains
- CONTEXT-DEPENDENT: Different answers for different contexts, with specific decision criteria

Weight by confidence x domain relevance:
- High confidence + Core domain: 1.0
- Medium confidence + Adjacent domain: 0.49
- Low confidence + Outside domain: 0.16

Never produce: "Both make valid points" or "It depends" without specifying decision factors.

## Final Report

Plain text with markdown headers where helpful. No box-drawing characters, no emoji, no decorative formatting.

Note which perspective was Codex-powered (different model family).

### Summary
1-2 sentences directly answering the question.

### Perspectives
List the roles that participated and which was Codex-powered.

### Consensus Points
Numbered list with consensus label for each.

### Key Trade-offs
For each: the trade-off, the recommendation, any dissent.

### Recommendations
Concrete, actionable items. Group by time horizon if relevant (immediate, short-term, ongoing).

### Open Questions
What remains unresolved and how to resolve it.

### Confidence Assessment
What the debate is confident about and what requires further investigation.
