# Perspective Selection Reference

## Approach

Derive domain-specific perspectives from the topic context. Do not use generic archetypes like "Skeptic" or "Pragmatist" as role labels. Instead, generate role labels that a real team would have for the specific problem being discussed.

## Topic Analysis

1. Extract domain keywords: technologies, frameworks, systems, constraint terms (security, performance, cost, UX).
2. Identify stakeholder angles: who implements, decides, uses, pays, maintains, secures.
3. Map conflict points: speed vs quality, innovation vs stability, cost vs capability, build vs buy.
4. Determine what domain expertise is needed to ground the discussion.

## Structural Roles

Two roles are always present to ensure debate mechanics work. Frame them in domain-specific terms.

**Contrarian**
- Challenges consensus and surfaces alternatives
- Frame in domain terms: for a migration debate, this might be "Core Data Advocate" who argues against migrating. For a build-vs-buy debate, this might be "Open Source Advocate" who challenges the buy assumption.
- The Contrarian's structural job is to prevent premature convergence. Its domain label should reflect the most productive opposing viewpoint for the specific topic.

**Synthesizer**
- Connects viewpoints and finds integration points beyond compromise
- Frame in domain terms: might be "iOS Platform Engineer" who sees the full system, or "Technical Program Manager" who bridges perspectives.
- The Synthesizer's structural job is to find higher-order understanding. Its domain label should reflect someone who naturally sees across the relevant domains.

## Domain-Specific Roles

The remaining 1-5 roles are derived entirely from the topic. For each, define:
- Role label (what a real team would call this person)
- Domain expertise (what they know deeply)
- What they care about (their optimization criteria)

## Examples

### "Should we migrate from Core Data to SwiftData?"

| Role | Domain | Cares about |
|------|--------|-------------|
| SwiftData Expert | New API, migration patterns, modern Apple data persistence | API capabilities, future direction |
| Core Data Advocate (Contrarian) | Legacy system, hidden complexity, production stability | Regression risk, known edge cases |
| Product Owner | Timeline, feature roadmap, user impact | Ship dates, feature parity during migration |
| QA Lead | Test coverage, regression detection, CI/CD | Test strategy, migration validation |
| iOS Platform Engineer (Synthesizer) | Full iOS stack, build systems, dependencies | System-wide coherence, migration path |

### "Build vs buy for notification system?"

| Role | Domain | Cares about |
|------|--------|-------------|
| Backend Infrastructure Engineer | Distributed systems, message queues, reliability | Scalability, operational burden |
| Product Manager | User engagement, notification UX, metrics | Delivery rates, user experience |
| Security/Compliance Lead | Data handling, vendor audits, regulatory | PII in notifications, audit trail |
| Vendor Evaluation Specialist | SaaS pricing, integration patterns, vendor lock-in | TCO, API quality, exit strategy |
| DIY Advocate (Contrarian) | Custom systems, competitive differentiation | Over-dependence on vendors |
| Technical Program Manager (Synthesizer) | Cross-team coordination, timelines | Feasibility, phased approach |

### "Why is checkout dropping 30% at payment?"

| Role | Domain | Cares about |
|------|--------|-------------|
| Payment Integration Engineer | Payment APIs, tokenization, error handling | API errors, timeout rates |
| UX Researcher | User behavior, funnel analysis, usability testing | Drop-off patterns, friction points |
| Frontend Performance Specialist | Load times, rendering, network requests | Page weight at payment step |
| Baseline Skeptic (Contrarian) | Industry benchmarks, funnel norms | Whether 30% is actually abnormal |

### "Should we adopt a 4-day work week?"

| Role | Domain | Cares about |
|------|--------|-------------|
| HR Operations Lead | Policy implementation, scheduling, compliance | Coverage gaps, overtime rules |
| Engineering Manager | Team productivity, sprint planning, delivery | Output measurement, meeting density |
| Customer Success Lead | Client coverage, response times, SLAs | Service continuity |
| Employee Wellbeing Researcher | Burnout, cognitive performance, retention | Wellbeing outcomes, work intensification |
| Status Quo Advocate (Contrarian) | Current operations, change risk | Disruption cost, competitive pressure |
| COO (Synthesizer) | Organization-wide operations | Phased rollout, measurement framework |

## Codex Perspective

One perspective is always powered by Codex CLI (a different model family). The team lead assigns Codex to whichever role benefits most from independent reasoning — typically a Specialist role or the Contrarian, since model diversity is most valuable where genuine disagreement matters.

The Codex perspective is a first-class team member. It is spawned as an Agent that relays prompts to `codex exec` via Bash. From the team lead's perspective, it is identical to any other agent.

## Format Rules

- Label by role only. No fictional names. No fictional titles like "Dr." or "Professor."
- No personality descriptions. No communication style notes.
- Keep statements direct and analytical. No "I think", no "in my experience as a...", no conversational filler.
- Each perspective's output should be labeled with its role: `Payment Integration Engineer: [statement]`
