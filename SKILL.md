---
name: debate
description: >
  Structured multi-perspective analysis using Hegelian dialectic. Generates
  anonymous archetype perspectives, facilitates multi-round debate, and
  synthesizes actionable conclusions. For architecture decisions, strategy,
  bug analysis, feature design, and general problem-solving.
license: MIT
metadata:
  version: "1.0"
---

# Structured Debate

Analyze a topic from multiple perspectives using structured rounds and Hegelian synthesis.

## Arguments

Parse `$ARGUMENTS`:
- `size:N` -> panel size override (3-5)
- `depth:X` -> quick (1 round), standard (2-3), deep (4+)
- Remaining text -> topic

## Flow

```
COMPLEXITY_CHECK -> PERSPECTIVE_SELECTION -> DISCUSSION_ROUNDS -> SYNTHESIS -> REPORT
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
| 5-7 | Warn: topic may not benefit from debate. Offer direct answer or proceed. |
| 8-11 | Standard debate: 3 perspectives, 2 rounds |
| 12-15 | Deep debate: 5 perspectives, 3+ rounds. Load all reference files. |

## Perspective Selection

Read [references/perspectives.md](references/perspectives.md) for detailed guidance.

Select anonymous archetypes based on the topic. Every debate must include:
1. Contrarian - challenges consensus, surfaces alternatives
2. Synthesizer - connects viewpoints, finds integration points
3. Specialist - deep domain knowledge, grounds the discussion

Additional archetypes (for 4-5 perspective panels): Skeptic, Pragmatist, Optimist, Theorist.

Label perspectives by archetype only. No names, no fictional backgrounds, no personality traits. Example: "Skeptic:", "Pragmatist:", not "Dr. Elena, the cautious security expert".

Panel size:
- Score 5-7: 3 perspectives (if proceeding)
- Score 8-11: 3 perspectives
- Score 12-15: 5 perspectives
- Override with `size:N`

## Clarifying Questions

At any point during the debate -- after complexity check, after perspective selection, or between rounds -- if the perspectives lack information needed to give a well-grounded analysis, use the AskUserQuestion tool to ask the user for clarification.

Guidelines:
- Ask when a key assumption would change the direction of the debate (e.g., team size, budget, timeline, existing constraints).
- Batch related questions into a single AskUserQuestion call (up to 4 questions).
- Phrase questions from the debate's perspective, not from a single archetype. Example: "The debate needs to know your current deployment frequency to assess migration risk."
- Propagate answers to all perspectives. After receiving clarification, briefly note the new information before continuing: "Given that the team is 8 engineers and deploys weekly, perspectives are updated accordingly."
- Do not ask questions that could be answered by reading the codebase or project context. Prefer reading files over asking.
- Do not over-ask. One round of clarification is usually sufficient. If more is needed, ask during a later synthesis when the gap becomes concrete.

## Discussion Rounds

### Round 1: Thesis (Opening Positions)

Each perspective states its position on the topic in 3-5 sentences. Direct, analytical, no conversational filler.

Format:
```
Specialist: [Position statement]
Skeptic: [Position statement]
Contrarian: [Position statement]
```

### Round 2+: Antithesis (Challenge and Response)

Perspectives challenge each other's positions. Patterns: claim vs counter-claim, question vs response, challenge vs defense.

Before any synthesis round, the Contrarian must be asked: "What are we missing?"

### Synthesis (Per-Round)

After each round of exchange, produce:

Agreements: Points where perspectives converge, noting whether they arrive via the same or different reasoning.

Tensions: Specific disagreements with the nature of the divergence (factual, values-based, methodological, scope).

Open questions: What remains unresolved and what would resolve it.

Emerging insights: Novel understanding that wasn't present in any single perspective.

Convergence rule: End early if all perspectives agree or if the debate is cycling. Extend if a major tension remains unexplored.

Validation: No "it depends" without specifying the concrete factors and decision criteria.

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

Structure:

### Summary
1-2 sentences directly answering the question.

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
