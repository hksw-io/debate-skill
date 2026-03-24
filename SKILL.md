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
  version: "2.1"
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
CONTEXT_GATHERING -> COMPLEXITY_CHECK -> PERSPECTIVE_SELECTION -> TEAM_SETUP -> PARALLEL_ROUNDS -> SYNTHESIS -> REPORT -> CLEANUP
```

## Context Gathering

Before scoring complexity or selecting perspectives, the team lead must build an understanding of the project and topic. This context directly informs which perspectives to bring into the debate.

### Codebase exploration

Use Glob, Grep, and Read to understand the project:
- **Platform detection**: Look for .xcodeproj/.xcworkspace (Apple), .csproj/.sln (Microsoft), AndroidManifest.xml (Android), package.json (Web). This determines whether to include a platform quality perspective.
- **Tech stack**: Read project config files, dependency manifests, and top-level source directories to understand what technologies are in play.
- **Relevant code**: If the topic references specific features, systems, or bugs, read the relevant source files. The team lead should understand the code under discussion before deciding who needs to debate it.
- **Project docs**: Read CLAUDE.md, README, or any architecture docs if they exist. These often contain constraints and decisions that matter for the debate.

### High-impact questions

After codebase exploration, evaluate whether there is enough context to select good perspectives. If there is — the topic is specific, the codebase provides clear context, and the desired outcome is obvious — proceed directly to perspective selection. If not, ask the user high-impact questions via AskUserQuestion before proceeding.

Ask about:
- **Desired outcome**: "What does a good result look like? A decision, a ranked list of options, a risk assessment?"
- **Constraints**: "Are there hard constraints the debate should respect? (timeline, budget, team size, technical debt limits, etc.)"
- **Scope**: "Is this about the immediate fix, the long-term architecture, or both?"
- **Examples**: "Can you give an example of what you mean by X?" — especially when the topic uses vague terms
- **Stakeholders**: "Who else cares about this decision? Who would push back on a change?"
- **Prior attempts**: "Has this been tried before? What happened?"

Pick the 2-4 most impactful questions for this specific topic. Do not ask generic questions that the codebase already answers. Do not ask more than 4.

The answers directly shape perspective selection — for example, if the user says "we tried this migration last year and rolled back," that warrants a perspective representing the lessons from that failure.

### Context summary

After gathering context, the team lead produces a brief context summary (not shown to the user — this is internal). It should capture:
- Platform and tech stack
- Relevant files and code areas
- Key constraints or decisions already made
- What the user is trying to achieve

This summary is included in every perspective agent's prompt so they start with shared context rather than having to rediscover it independently.

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

**Platform quality perspective:** When the codebase targets a specific platform, include a platform quality perspective. See [references/perspectives.md](references/perspectives.md) for details. Examples:
- Apple/iOS/macOS projects: "Apple Platform Quality" — evaluates against Apple HIG, wording standards, animation polish, progressive disclosure, typographic attention
- Microsoft/.NET/Windows projects: "Microsoft Platform Quality" — evaluates against Fluent Design, accessibility standards, enterprise UX patterns, inclusive design
- Android projects: "Material Design Quality" — evaluates against Material Design guidelines, adaptive layouts, motion semantics
- Web projects: "Web Standards Quality" — evaluates against WCAG, performance budgets, progressive enhancement

This perspective is triggered by detecting the platform from the codebase (Xcode project files, .csproj, Android manifests, etc.) or from the topic itself.

**One perspective is always powered by Codex CLI.** The team lead assigns it to whichever role fits best. This provides genuine model diversity — a different model family reasoning about the same problem.

No fictional names. No fictional backgrounds. No personality traits. Label by role only.

## Clarifying Questions

Actively ask questions to make sure the debate is working on the right thing. Questions can come from the team lead, from any perspective agent, or emerge during synthesis. **All questions must be resolved before proceeding to the next round.**

### Who can ask

**Team lead** asks the user directly via AskUserQuestion. This is the primary channel.

**Perspective agents** can include questions in their round output under a markdown header. The team lead scans agent outputs for this section, collects the questions, and asks the user via AskUserQuestion before starting the next round.

Agent output format for questions:
```
# Open Questions
1. Can you give an example of the quality issues you're seeing?
2. Is the current Core Data schema the source of truth, or is it changing?
```

### When to ask

- **Before Round 1**: If the topic is ambiguous, missing context, or could be interpreted multiple ways. Ask for examples, constraints, or the specific outcome the user is trying to achieve. Do not guess — ask.
- **Between rounds**: If agents identify gaps in their analysis. For example, "We don't know the team size, which would change whether we recommend approach A or B."
- **During synthesis**: If tensions cannot be resolved without user input on priorities or constraints.

### What to ask about

- **Missing context**: team size, budget, timeline, existing constraints, technical stack details
- **Ambiguous scope**: "Are you asking about the migration itself or the long-term architecture?"
- **Examples**: "Can you give an example of the kind of quality issue you're seeing?" or "What does a good outcome look like?"
- **Priorities**: "The debate has a tension between speed and thoroughness. Which matters more for this decision?"
- **Verification**: "We found X in the codebase. Is this the current state, or is it changing?"

### How to ask

- Batch related questions into a single AskUserQuestion call (up to 4 questions).
- Phrase questions specifically: "The debate needs to know your current deployment frequency to assess migration risk" — not "Do you have any other context?"
- Prefer reading codebase files over asking. Do not ask what you can look up.

### Propagation

After receiving answers, the team lead:
1. Sends the user's answers to all perspective agents via SendMessage.
2. Briefly summarizes: "User clarified: team is 8 engineers, deploys weekly, codebase is 3 years old."
3. Only then proceeds to the next round. **Never start a round while questions are pending.**

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
- `round-2-response`: each perspective responds to others (focused on specific tensions from Round 1 synthesis)
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
> Project context: [CONTEXT SUMMARY — platform, tech stack, relevant files, key constraints, what the user is trying to achieve]
>
> Round [N] instructions: [STATE OPENING POSITION / RESPOND TO OTHERS]
>
> [For Round 2+: Here are all perspectives from the previous round: ...]
> [For Round 2+: The synthesis identified these key tensions to address: ...]
>
> Quality rules:
> - **Ground your claims.** Before stating your position, search the codebase for relevant code, configs, tests, or docs using Read and Grep. Cite specific files and lines when they support your argument. If you cannot find evidence, say so.
> - **Tag your evidence.** Mark each claim as: [grounded] if verified in code/docs, [informed] if based on domain knowledge, or [speculative] if uncertain.
> - **Steelman before attacking.** When disagreeing with another perspective, first state the strongest version of their argument, then explain why you still disagree.
> - **State your falsifiability.** For your key claims, state what evidence or outcome would change your mind.
> - **Distinguish knowledge from assumption.** Be explicit about what you know vs what you're assuming.
> - **Ask if unsure.** If you lack context needed for a well-grounded position, add an `# Open Questions` section at the end of your output with a numbered list of questions. The team lead will ask the user and share the answer before the next round. Do not guess when you can ask. Ask for examples if the topic is abstract.
> - Be direct and analytical. No conversational filler.
> - Label your output with your role name.
> - Check TaskList for your assigned task and mark it complete when done.

**Codex perspective agent prompt template:**

Same role/topic/round information and quality rules as any other agent, plus this instruction:

> You do not reason about this topic yourself. Instead, for each round:
> 1. Take your role description, the topic, the quality rules, and any prior round statements
> 2. Construct a prompt that captures all of this context
> 3. Run it via Bash: `/opt/homebrew/bin/codex exec -s read-only --full-auto 'PROMPT'`
> 4. Return the Codex output as your position statement
>
> You are a relay between the debate and Codex CLI. Participate in the task list and messaging like any other agent.

### Step 4: Execute rounds

All agents are treated identically by the team lead:

**Round 1 (Thesis):** All agents work their opening position task in parallel. Team lead collects all outputs. Before proceeding to synthesis, the team lead scans all outputs for `# Open Questions` sections, batches the questions, asks the user via AskUserQuestion, and propagates answers to all agents. **Do not proceed to synthesis or Round 2 while questions are pending.**

**Round 2+ (Antithesis):** Team lead sends all positions to each agent via SendMessage, **along with the specific tensions identified in the synthesis**. Agents are directed to address these tensions rather than broadly "respond to others." All agents work in parallel. Before synthesis, the Contrarian is asked: "What are we missing?" Again, collect and resolve any `# Open Questions` from agent outputs before proceeding.

**Per-round synthesis:**

After collecting all round outputs, the team lead first asks the Synthesizer agent to produce a draft synthesis. The Synthesizer should identify agreements, tensions, and integration points from its independent perspective. The team lead then refines the Synthesizer's draft into the final per-round synthesis:

- Agreements: where perspectives converge, noting same vs different reasoning
- Tensions: specific disagreements categorized as factual, values-based, methodological, or scope
- Open questions: what remains unresolved and what would resolve it
- Emerging insights: novel understanding not present in any single perspective
- Surprise finding: the single most non-obvious or unexpected insight from this round — something the user likely didn't consider when posing the question
- Evidence quality: note which claims are grounded (verified in code), informed (domain knowledge), or speculative
- Codex consultations: if used this round, what was asked and learned
- Claude vs Codex divergence: if the Codex perspective diverged meaningfully from Claude perspectives, note where and why — this cross-model disagreement is often the most interesting signal

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

### Decision
If the topic is a decision (most are): "If you must decide now, do X because Y." One clear recommendation with the primary justification. This is not a hedge — commit to the position best supported by the debate. If the debate is genuinely contested, say so and state the tiebreaker criteria.

### Perspectives
List the roles that participated and which was Codex-powered.

### Consensus Points
Numbered list with consensus label for each.

### Key Trade-offs
For each: the trade-off, the recommendation, any dissent.

### Surprise Finding
The single most non-obvious or unexpected insight from the debate — something the user likely didn't consider when posing the question. This is often the most valuable output.

### Strongest Dissent
Steelman the best minority argument. Present the strongest version of the dissenting view, who held it, and under what conditions it would be the right choice. Do not dismiss it — the minority view often contains the most valuable signal.

### Claude vs Codex
Where the Codex-powered perspective diverged from Claude perspectives. What does cross-model disagreement reveal about the certainty of the conclusions?

### Evidence Quality
Summary of how well-grounded the debate's claims are:
- Grounded claims (verified in code/docs): list key ones
- Informed claims (domain knowledge): list key ones
- Speculative claims (uncertain): list key ones and what would verify them

### Recommendations
Concrete, actionable items. Group by time horizon if relevant (immediate, short-term, ongoing).

### Open Questions
What remains unresolved and how to resolve it.

### Confidence Assessment
What the debate is confident about and what requires further investigation.
