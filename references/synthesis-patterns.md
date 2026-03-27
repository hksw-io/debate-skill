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

**Agreements:** Explicit (same conclusion and reasoning) or implicit (same conclusion, different reasoning paths). Tag implicit agreements as convergent — they strengthen confidence when multiple domains validate the same point. When an agreement reveals a novel insight not present in any single perspective, note it here.

**Tensions:** Categorize each disagreement:
- Factual: different empirical claims or predictions. Resolution: identify evidence that would settle it.
- Values-based: different optimization criteria or stakeholder priorities. Resolution: make trade-offs explicit.
- Definitional: different meanings for key terms. Resolution: standardize or acknowledge multiple framings.
- Methodological: different approaches to the same problem. Resolution: identify contexts where each excels.
- Scope: different problem boundaries. Resolution: explicitly define scope.

Note when a tension stems from grounded vs speculative claims — a factual disagreement where one side has [grounded] evidence and the other has [speculative] claims is weighted differently than one where both sides are grounded.

**Surprise finding:** The single most non-obvious or unexpected insight from this round. Ask: "What would the user not have considered when they posed this question?" If the surprise comes from Codex diverging from Claude perspectives, say so. The surprise finding is often the most valuable output of the debate — it justifies the cost of running multiple agents.

**Directions for next round:** What tensions to explore, what evidence to gather, any Codex consultations to request. Forward-looking guidance, not backward-looking summary. Template: "In the next round, [Agent] should address [specific tension] by [concrete action]."

## Consensus Labels

UNANIMOUS: All perspectives agree on conclusion and reasoning. High confidence.

STRONG: All agree on conclusion via different reasoning paths. High confidence, reinforced by convergence.

MAJORITY: Most perspectives agree. Minority view must be acknowledged with reasoning. Avoid suppressing minority corrections.

CONTESTED: Fundamental disagreement. May be irreducible due to values or genuine uncertainty. Present both positions with conditions favoring each.

CONTEXT-DEPENDENT: Different answers for different contexts. Not a failure but a recognition of complexity. Must specify the exact factors and provide a decision framework.

## Weighted Synthesis

When perspective relevance varies by sub-question, weight accordingly:

On sub-question A where Specialist has core domain expertise, weight their view highest. On sub-question B where Pragmatist's operational knowledge dominates, weight theirs. Always explain why a particular perspective's view is weighted higher on a given point.

Weight perspectives by relevance to the specific sub-question. A specialist's view on their core domain should dominate over an adjacent domain's opinion, which in turn should dominate over out-of-domain speculation. Discount speculative claims from perspectives operating outside their domain. Always explain why a particular perspective is weighted higher on a given point.

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
1. After collecting all round outputs, send them to the Synthesizer agent with: "Produce a draft synthesis: agreements, tensions, surprise finding, and directions for next round."
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
