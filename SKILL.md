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
  version: "3.0"
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

If the topic is not about code (e.g., organizational decisions, strategy, hiring, process), skip codebase exploration. Proceed directly to high-impact questions. The rest of the debate flow applies unchanged.

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

Score the topic on 3 dimensions (1-3 each):

| Dimension | 1 (Low) | 2 (Medium) | 3 (High) |
|-----------|---------|------------|----------|
| Trade-offs | Clear winner | 1-2 trade-offs | 3+ competing trade-offs |
| Reversibility | Easily reversed | Partially reversible | Irreversible or very costly |
| Domain breadth | Single domain | 2 domains | 3+ domains |

Total = sum (range 3-9).

| Score | Action |
|-------|--------|
| 3-4 | Warn: topic may be simple. If user proceeds: 3 perspectives, 1-2 rounds |
| 5-6 | Standard: 3-5 perspectives, 2 rounds |
| 7-9 | Deep: 5-7 perspectives, 3+ rounds. Load all reference files. |

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

Questions can come from the team lead (during context gathering) or from perspective agents (during rounds). **All material questions must be resolved before proceeding to the next round.**

### Team lead questions

The team lead asks the user directly via AskUserQuestion during context gathering if the topic is ambiguous or missing context. Prefer reading codebase files over asking — do not ask what you can look up.

### Agent questions (structured format)

Perspective agents communicate questions by including a `# Questions for User` section at the end of their output. Every agent MUST include this section, even when they have no questions.

**Format when agent has questions:**
```
# Questions for User

1. [QUESTION] Can the database schema be modified, or is it shared with other services?
   [WHY] My position on migration risk depends on whether schema changes are possible.
   [ASSUME_IF_NO_ANSWER] Schema is shared and cannot be modified.

2. [QUESTION] What is the team's experience with SwiftData?
   [WHY] This affects whether I recommend a phased migration or a full rewrite.
   [ASSUME_IF_NO_ANSWER] Team has no prior SwiftData experience.
```

**Format when agent has no questions:**
```
# Questions for User

NONE
```

### Propagation

After receiving answers, the team lead:
1. Sends the user's answers to all perspective agents via SendMessage.
2. Briefly summarizes: "User clarified: team is 8 engineers, deploys weekly, codebase is 3 years old."
3. Only then proceeds to the next round. **Never start a round while questions are pending.**

## Codex Consultation

During Round 2+, any Claude perspective agent can request an ad-hoc Codex consultation for a specific factual or technical question. This is separate from the mandatory Codex perspective — it's for when an agent wants to verify a specific claim.

- Max 2 ad-hoc consultations per debate.
- The requesting agent must state what it wants to learn and why.
- Invoke via the Skill tool: `skill: "codex", args: "reasoning:EFFORT PROMPT"`
- Set reasoning effort based on the debate's complexity score (see reasoning effort table below).
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
> [For Round 2+: Full cumulative debate history follows — all prior rounds, not just the last one]
> [Round 1 positions: all perspectives' opening statements]
> [Round 1 synthesis: agreements, tensions, surprise finding, directions]
> [User clarifications: any answers from the user between rounds]
> [Round 2 positions: ...] (if Round 3+)
> [Round 2 synthesis: ...] (if Round 3+)
> [For Round 3+: Summarize Round 1 positions to 2-3 sentences each. Include Round 2+ in full.]
> [Codex consultation results: ...] (if any were used)
> [Key tensions to address this round: ...]
>
> Quality rules:
> 1. **Ground and tag.** Search the codebase for relevant code, configs, tests, or docs using Read and Grep. Cite specific files and lines. Tag each claim: [grounded] if verified in code/docs, [informed] if based on domain knowledge, [speculative] if uncertain — state what would verify it. If you cannot find evidence, say so.
> 2. **Steelman before attacking.** When disagreeing with another perspective, first state the strongest version of their argument, then explain why you still disagree.
> 3. **Ask if unsure.** If you lack context for a well-grounded position, you MUST add a `# Questions for User` section at the end of your output using the structured format (see Clarifying Questions section). State what you know vs assume. If you have no questions, end with `# Questions for User` followed by `NONE`. **Do NOT call AskUserQuestion yourself — only the team lead does. Calling it from a teammate agent will deadlock the debate.**
> 4. **Be direct.** No conversational filler. Label output with your role name. Check TaskList and mark your task complete when done.
>
> For software engineering topics, prefer current APIs and modern approaches over deprecated or legacy ones — search results often surface outdated patterns.

**Codex reasoning effort table:**

The team lead always sets reasoning effort for every Codex invocation (both the Codex perspective agent and ad-hoc consultations). This is not optional.

| Complexity Score | Reasoning Effort |
|-----------------|-----------------|
| 3-4 (simple) | medium |
| 5-6 (standard) | high |
| 7-9 (deep) | xhigh |

**Codex perspective agent prompt template:**

Same role/topic/round information and quality rules as any other agent, plus this instruction:

> You do not reason about this topic yourself. Instead, for each round:
> 1. Take your role description, the topic, the quality rules, and the full debate history
> 2. Construct a single prompt that includes: (a) your role and what you care about, (b) the topic, (c) the 4 quality rules, (d) the full debate history from prior rounds, and (e) the specific tensions to address this round. Structure the prompt so Codex can act on it without needing additional context. Do not send multiple prompts — one comprehensive prompt per round.
> 3. Invoke it via the Skill tool: `skill: "codex", args: "reasoning:EFFORT PROMPT"`
>    where EFFORT is set by the team lead based on the complexity score (see table above).
> 4. Return the Codex output as your position statement
> 5. Your output must end with a `# Questions for User` section. Parse Codex's output for any questions and include them in the structured format. If Codex asked none, output `# Questions for User` followed by `NONE`.
>
> You are a relay between the debate and Codex CLI. Participate in the task list and messaging like any other agent.

### Step 4: Execute rounds

All agents are treated identically by the team lead:

**Round 1 (Thesis):** All agents work their opening position task in parallel. Team lead collects all outputs, then follows the **mandatory open questions check** below before proceeding.

**Round 2+ (Antithesis):** Team lead sends each agent the **full cumulative debate history** via SendMessage — all prior rounds' positions, all syntheses, all user clarifications, all Codex consultation results — plus the specific tensions to address this round. Agents are directed to address these tensions rather than broadly "respond to others." This is especially critical for the Codex agent, which is stateless (each `codex exec` is a fresh invocation and has no memory of prior rounds). All agents work in parallel. After collecting outputs, follow the **mandatory questions check** below before proceeding.

**Agent failure:** If an agent fails to respond or produces an error, note the gap in the synthesis and proceed with the remaining perspectives. Do not retry more than once.

### Mandatory questions check (after EVERY round)

**This step is NOT optional.** After collecting all agent outputs for a round, the team lead MUST execute this protocol:

1. **Scan every agent output** for the `# Questions for User` section.
   - If any agent's output lacks this section entirely, send that agent a follow-up message: "Your output is missing the `# Questions for User` section. Reply with your questions or `NONE`." Wait for the response before proceeding.
2. **Collect all non-NONE questions** into a single numbered list with attribution (which agent asked).
3. **Apply materiality filter.** For each question, apply the counterfactual test: "If the user gave the opposite answer, would it change the debate's recommendation?" Drop questions that fail this test.
4. **If material questions remain:**
   a. Batch into AskUserQuestion calls (up to 4 questions per call, multiple calls if needed). **MUST use AskUserQuestion tool — do not ask questions via text output.**
   b. **STOP and WAIT for the user's answer.** Do not proceed to synthesis or the next round.
   c. After receiving answers, propagate to all agents via SendMessage before continuing.
   d. If the user declines to answer a specific question, use the agent's [ASSUME_IF_NO_ANSWER] and propagate that assumption.
5. **If no material questions remain:** Proceed to synthesis / next round.

**Per-round synthesis:**

After collecting all round outputs, the team lead first asks the Synthesizer agent to produce a draft synthesis. The Synthesizer should identify agreements, tensions, and integration points from its independent perspective. The team lead then refines the Synthesizer's draft into the final per-round synthesis:

- **Agreements**: where perspectives converge, noting same vs different reasoning. When an agreement reveals a novel insight not present in any single perspective, note it here.
- **Tensions**: specific disagreements categorized as factual, values-based, methodological, or scope. Note when a tension stems from grounded vs speculative claims.
- **Surprise finding**: the single most non-obvious or unexpected insight from this round — something the user likely didn't consider. If the surprise comes from Codex diverging from Claude perspectives, say so.
- **Directions for next round**: what tensions to explore, what evidence to gather, any Codex consultations to request.

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

Weight perspectives by relevance to the specific sub-question. On a technical question about SwiftData migration, weight the SwiftData Expert highest. On a question about ship timeline, weight the Product Owner highest. Always explain why a particular perspective's view carries more weight on a given point. Discount speculative claims from perspectives operating outside their domain.

Never produce: "Both make valid points" or "It depends" without specifying decision factors.

## Final Report

Plain text with markdown headers where helpful. No box-drawing characters, no emoji, no decorative formatting.

**Core sections (always present):**

### Decision
"If you must decide now, do X because Y." One clear recommendation with the primary justification. This is not a hedge — commit to the position best supported by the debate. If the debate is genuinely contested, say so and state the tiebreaker criteria. Note which perspectives participated and which was Codex-powered (different model family).

### Key Trade-offs
For each trade-off: the trade-off, the recommendation, any dissent, and a consensus label (UNANIMOUS / STRONG / MAJORITY / CONTESTED / CONTEXT-DEPENDENT). Inline the label rather than listing consensus points separately.

### Recommendations
Concrete, actionable items. Group by time horizon if relevant (immediate, short-term, ongoing).

### Open Questions / Next Steps
What remains unresolved, how to resolve it, and what the user should investigate next.

**Conditional sections (include only when substantive):**

### Surprise Finding
Include if the debate produced a non-obvious insight the user likely did not consider. Omit if nothing genuinely surprising emerged — do not manufacture one.

### Strongest Dissent
Include if there was meaningful minority disagreement. Steelman it: present the strongest version of the dissenting view, who held it, and under what conditions it would be the right choice. Omit if consensus was unanimous and no dissent had merit.

### Cross-Model Divergence
Include if the Codex-powered perspective diverged meaningfully from Claude perspectives. What does the disagreement reveal about certainty? Omit if Codex and Claude agreed.
