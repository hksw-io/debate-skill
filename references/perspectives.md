# Perspective Selection Reference

## Topic Analysis

1. Extract domain keywords: technical terms, frameworks, constraint keywords (security, performance, cost, UX).
2. Classify topic breadth:
   - Narrow: single technology or specific implementation (3 perspectives)
   - Medium: cross-functional decision (3-4 perspectives)
   - Broad: strategic or organizational change (5 perspectives)
3. Identify stakeholder angles: who implements, decides, uses, pays, maintains, secures.
4. Map conflict points: speed vs quality, innovation vs stability, cost vs capability.

## Archetypes

### Required (every debate)

Contrarian
- Challenges consensus and groupthink
- Explores unconventional approaches
- Questions unstated assumptions

Synthesizer
- Connects ideas across perspectives
- Finds integration points beyond compromise
- Clarifies misunderstandings between perspectives

Specialist
- Deep expertise in the relevant domain
- Provides technical accuracy and grounding
- Knows domain-specific context and constraints

### Additional

Optimist
- Focuses on opportunities and potential
- Highlights benefits and positive outcomes
- Risk: may underestimate challenges

Skeptic
- Questions assumptions and claims
- Identifies risks and failure modes
- Demands evidence
- Risk: may block progress with over-caution

Pragmatist
- Balances ideals with constraints
- Focuses on what's achievable given resources and timelines
- Risk: may settle for suboptimal solutions

Theorist
- Emphasizes principles and best practices
- Thinks long-term and systematically
- Risk: may be disconnected from practical realities

### Secondary Traits (combinable with any archetype)

- Data-driven: wants metrics, benchmarks, studies
- User-centric: returns to end-user impact
- Cost-conscious: calculates TCO, ROI
- Risk-averse: prioritizes safety and reliability
- Process-oriented: emphasizes methodology and governance

## Diversity Rules

Requirements:
- All 3 required archetypes present
- No single archetype more than 30% of panel
- At least 2 different knowledge domains for medium+ breadth topics
- Perspectives should create productive tension, not just agreement

## Selection by Topic Type

Narrow technical topics (e.g., "PostgreSQL JSONB vs separate tables"):
- 2-3 specialists with different sub-domain focus
- 1 contrarian/skeptic
- Keep it tight; don't over-staff simple questions

Cross-functional decisions (e.g., "Should we adopt microservices?"):
- 1-2 specialists (different domains)
- 1 skeptic or pragmatist
- 1 synthesizer
- Optional: business/cost perspective

Broad strategic topics (e.g., "Build vs buy CRM"):
- 1-2 specialists
- 1 strategic/theorist
- 1 skeptic/contrarian
- 1 synthesizer
- 1 user/customer or business perspective

Bug analysis and debugging:
- Specialist (in the relevant system area)
- Skeptic (challenges initial assumptions about root cause)
- Pragmatist (focuses on fix feasibility and risk)

Feature design:
- Specialist (technical feasibility)
- User-centric perspective (user impact and experience)
- Contrarian (challenges whether the feature is needed at all)

## Format

Always use archetype label only. Never assign names, titles, or fictional backgrounds.

Good:
```
Skeptic: The migration timeline assumes zero regression, which contradicts our last three deployments.
```

Bad:
```
Dr. Sarah Chen (Security Architect): Well, I've been in security for 20 years, and I have to say...
```

Keep statements direct and analytical. No conversational filler, no "I think", no "in my experience as a...". Just the analysis.
