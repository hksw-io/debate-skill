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

## Platform Quality Perspective

When the codebase or topic targets a specific platform, include a platform quality perspective. This role evaluates the debate's proposals and recommendations against the platform's design ethos and quality standards.

Detect the platform from:
- Codebase files: .xcodeproj/.xcworkspace (Apple), .csproj/.sln (Microsoft), AndroidManifest.xml (Android), package.json with web frameworks (Web)
- The topic itself: mentions of iOS, macOS, SwiftUI, .NET, Android, React, etc.

### Apple Platform Quality

Role label: "Apple Platform Quality"

Evaluates against:
- Human Interface Guidelines (HIG): layout, navigation, controls, system integration
- Wording and copy: Apple's tone — concise, helpful, no jargon, sentence case, specific verbs
- Visual polish: spacing, alignment, typography hierarchy, SF Symbols usage, animation timing
- Progressive disclosure: showing the right information at the right time, not overwhelming users
- Platform integration: using system features (Spotlight, Shortcuts, Widgets, ShareSheet) where appropriate
- Accessibility: VoiceOver, Dynamic Type, color contrast, motion sensitivity

Cares about: does this feel like it belongs on an Apple platform? Would Apple's design review team approve this?

### Microsoft Platform Quality

Role label: "Microsoft Platform Quality"

Evaluates against:
- Fluent Design System: depth, motion, material, scale, light
- Accessibility: WCAG AA minimum, high contrast themes, narrator support, keyboard navigation
- Enterprise UX: information density, bulk operations, discoverability for power users
- Inclusive design: designing for permanent, temporary, and situational disabilities
- Platform conventions: command bar patterns, pane navigation, settings paradigms

Cares about: does this meet enterprise-grade quality? Is it accessible and inclusive by default?

### Material Design Quality (Android)

Role label: "Material Design Quality"

Evaluates against:
- Material Design 3: dynamic color, typography scale, shape system
- Adaptive layouts: phone, tablet, foldable, large screen
- Motion semantics: container transforms, shared axis, fade through
- System integration: notifications, widgets, app actions
- Accessibility: TalkBack, Switch Access, font scaling

Cares about: does this follow Material conventions and adapt across form factors?

### Web Standards Quality

Role label: "Web Standards Quality"

Evaluates against:
- WCAG 2.1 AA compliance
- Performance budgets: Core Web Vitals (LCP, INP, CLS)
- Progressive enhancement: works without JavaScript, enhanced with it
- Responsive design: mobile-first, fluid typography, container queries
- Semantic HTML: proper heading hierarchy, landmarks, ARIA when needed

Cares about: is this accessible, performant, and progressively enhanced?

## Codex Perspective

One perspective is always powered by Codex CLI (a different model family). The team lead assigns Codex to whichever role benefits most from independent reasoning — typically a Specialist role or the Contrarian, since model diversity is most valuable where genuine disagreement matters.

The Codex perspective is a first-class team member. It is spawned as an Agent that invokes the `codex` skill via the Skill tool. From the team lead's perspective, it is identical to any other agent.

## Format Rules

- Label by role only. No fictional names. No fictional titles like "Dr." or "Professor."
- No personality descriptions. No communication style notes.
- Keep statements direct and analytical. No "I think", no "in my experience as a...", no conversational filler.
- Each perspective's output should be labeled with its role: `Payment Integration Engineer: [statement]`
