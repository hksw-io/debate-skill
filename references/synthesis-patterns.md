# Synthesis Patterns Reference

## Hegelian Dialectic

Thesis: A position grounded in specific reasoning and evidence.
Antithesis: A competing position that reveals limitations in the thesis. Not mere negation but an alternative perspective, also grounded in evidence.
Synthesis: Preserves valid elements from both. Transcends simple compromise. Generates new understanding not present in either original position.

Key principles:
- Each synthesis both cancels and preserves elements of thesis/antithesis
- Synthesis should contain insights that neither position held individually
- Different contexts may favor different positions
- Each round's synthesis becomes the next round's thesis

## Per-Round Synthesis

After each round, produce four categories:

Agreements: Explicit (same conclusion and reasoning) or implicit (same conclusion, different reasoning paths). Tag implicit agreements as convergent -- they strengthen confidence when multiple domains validate the same point.

Tensions: Categorize each disagreement:
- Factual: different empirical claims or predictions. Resolution: identify evidence that would settle it.
- Values-based: different optimization criteria or stakeholder priorities. Resolution: make trade-offs explicit.
- Definitional: different meanings for key terms. Resolution: standardize or acknowledge multiple framings.
- Methodological: different approaches to the same problem. Resolution: identify contexts where each excels.
- Scope: different problem boundaries. Resolution: explicitly define scope.

Open questions: Must point to specific unknowns, suggest concrete resolution paths, and be necessary for progress. Template: "To resolve [tension], we need to determine [specific unknown]. This could be answered by [concrete action]."

Emerging insights: Look for hidden assumptions made explicit, reframings of the problem, novel combinations of perspectives, and contextual boundaries where different positions apply.

Surprise finding: After completing the synthesis, identify the single most non-obvious or unexpected insight from this round. Ask: "What would the user not have considered when they posed this question?" The surprise finding is often the most valuable output of the debate — it justifies the cost of running multiple agents.

Evidence quality: Track the grounding of claims across perspectives:
- [grounded]: verified in code, docs, configs, or test results — cite the file and line
- [informed]: based on domain knowledge or established best practices — state the source
- [speculative]: uncertain or assumed — state what would verify it
If a round's claims are mostly speculative, flag this and consider directing agents to do more codebase research in the next round.

Claude vs Codex divergence: When the Codex-powered perspective reaches a different conclusion than the Claude perspectives, this is a high-value signal. Note:
- What specifically diverged (conclusion, reasoning, evidence cited)
- Whether the divergence reveals a genuine uncertainty or a model-specific bias
- Whether it changes the confidence level of the consensus

## Consensus Labels

UNANIMOUS: All perspectives agree on conclusion and reasoning. High confidence.

STRONG: All agree on conclusion via different reasoning paths. High confidence, reinforced by convergence.

MAJORITY: Most perspectives agree. Minority view must be acknowledged with reasoning. Avoid suppressing minority corrections.

CONTESTED: Fundamental disagreement. May be irreducible due to values or genuine uncertainty. Present both positions with conditions favoring each.

CONTEXT-DEPENDENT: Different answers for different contexts. Not a failure but a recognition of complexity. Must specify the exact factors and provide a decision framework.

## Weighted Synthesis

When perspective relevance varies by sub-question, weight accordingly:

On sub-question A where Specialist has core domain expertise, weight their view highest. On sub-question B where Pragmatist's operational knowledge dominates, weight theirs. Always explain why a particular perspective's view is weighted higher on a given point.

Confidence x domain relevance weighting:
- High confidence + Core domain: 1.0
- Medium confidence + Adjacent domain: 0.49
- Low confidence + Outside domain: 0.16

## Handling Unresolved Tensions

Legitimate reasons for persistent disagreement:
- Genuine trade-offs with no single right answer
- Different stakeholders with different needs
- Insufficient evidence to resolve an empirical question
- Multiple valid value frameworks

For each unresolved tension, provide:
- Both positions with what each optimizes for
- The context where each is strongest
- The risk of choosing each
- A decision framework: choose A if [criteria], choose B if [criteria]

## Constructive "It Depends"

Bad: "It depends on your situation."
Good: "The answer depends on three factors: [factor 1], [factor 2], [factor 3]. If [condition], then [recommendation]. If [other condition], then [other recommendation]."

When possible, provide a decision matrix mapping factor combinations to recommendations with confidence levels.

## Dissent Preservation

The minority view often contains the most valuable signal. When the debate produces a majority or strong consensus, the synthesis must steelman the strongest dissenting argument:

1. State the dissenting position in its strongest possible form — better than the dissenter stated it themselves if possible.
2. Identify the conditions under which the dissent would be correct.
3. Name the specific risk of ignoring the dissent.

Do not dismiss dissent with "while this is a valid concern..." followed by moving on. If a dissent has merit under specific conditions, those conditions must be in the final report.

Dissent is especially valuable when:
- It comes from the Codex perspective (cross-model disagreement)
- It identifies a risk that the majority is systematically underweighting
- It reframes the question in a way the majority hasn't considered

## Synthesizer Agent Role in Synthesis

The Synthesizer agent (one of the two structural roles) produces a draft synthesis after each round. The team lead then refines it. This matters because:
- The Synthesizer has reasoned independently about the topic — its synthesis reflects genuine understanding, not just aggregation
- The team lead can check the Synthesizer's draft against the raw agent outputs to catch any blind spots
- If the Synthesizer and team lead disagree on the synthesis, that itself is an insight worth noting

Process:
1. After collecting all round outputs, send them to the Synthesizer agent with: "Produce a draft synthesis: agreements, tensions, open questions, emerging insights, and your surprise finding."
2. The Synthesizer returns its draft.
3. The team lead refines the draft into the final per-round synthesis, noting any adjustments.

## Anti-Patterns

Vague integration: "Both make valid points" without specifying where and why.
Ignoring disagreement: Claiming consensus when perspectives fundamentally disagree.
Premature consensus: Resolving complex questions in one round without exploring tensions.
Abstraction without grounding: "Adopt a holistic approach" without concrete actions.
Expertise mismatch: Treating all perspectives equally when a question clearly falls in one domain.

## Quality Checklist

Before finalizing any synthesis:
- Does it provide specific next steps, not just observations?
- Are important caveats and boundary conditions included?
- If it says "it depends," does it specify on what and how to decide?
- Does it contain insights not present in any single perspective?
- Are domain-specific insights weighted appropriately?
- Are unresolved issues stated with resolution paths?
- Does it actually answer what was asked?
