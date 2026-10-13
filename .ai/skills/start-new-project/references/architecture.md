# Requirement-driven architecture decisions

Use for phase 23 after the product/domain and MVP are sufficiently understood. Read `.ai/rules/architecture.md` and all applicable rule files first. This reference is a decision process, not a parallel Laravel manual.

## Readiness

Before proposing architecture establish primary actors/workflows, record ownership, essential invariants, data lifecycle, expected load, integration dependencies and MVP priorities. If a critical fact is missing, return to discovery.

Classify every choice as inherited baseline, Accepted project decision, Proposed or Open question. Explain the simplest suitable option, functional consequences and credible alternatives. Do not ask users to select pattern names they cannot evaluate.

## Decision map

| Choice | Functional evidence to seek | Restraint |
| --- | --- | --- |
| Monolith / services | Independent ownership, lifecycle/deployability or integration boundary | Start in existing Laravel app; SaaS/scale alone does not justify microservices |
| Inertia / exposed API | Browser app, partner/mobile clients or independently consumed contract | Preserve Vue/Inertia for application UI; a required API may coexist; never suggest Livewire |
| Database | Deployment constraint, data/query needs, recovery and engine-specific features | Supported config/test engine is not a confirmed production choice |
| Tenancy | Isolation, membership, sharing, administration, legal/performance boundaries | Do not infer separate databases or a tenancy package from companies |
| Authorization | Actor/record/action/condition matrix | Ownership/policies may suffice; do not install RBAC by habit |
| Jobs / scheduler | Allowed delay, retries, timed triggers and failure recovery | No queue job for quick synchronous behavior; installed driver does not prove a worker |
| Cache | Repeated expensive work, acceptable staleness and invalidation | No speculative Redis/cache layer; preserve permission/tenant context |
| Files | Sensitivity, size, access, retention and processing | Use existing storage capability; provider/scan decisions need evidence |
| Search | Confirmed matching/scale/index-lag requirements | SQL first when sufficient; justify external engine operational cost |
| Integrations | Mapping, quotas, resilience, ownership and contract | Focused boundaries; no repository per Eloquent model |
| Audit/history | Provenance, old-state reconstruction or tamper requirement | No event sourcing/history framework without need |
| Observability | Who detects/responds to failures, service expectations | Existing logging/Sentry wiring is not an alerting plan |

## Abstraction justification

For every proposed Action, Service, repository/adapter, DTO, Query Object, Event, Listener, Job or new module, state the actual requirement it serves and the concrete complexity it removes. Apply the existing rules' use/do-not-use conditions.

Omit abstractions without such a reason. It is valid for initial architecture to use controllers, Form Requests, policies and Eloquent directly. Functional domain names do not require domain directory scaffolding. Avoid prescribing an Action package or invocation convention absent local evidence.

## Flows and consistency

Describe the main request as input → authentication/record scope → authorization/validation → business operation → persistence/commit → response and permitted side effects, adjusted to the confirmed workflow. Do not pretend every simple read has every stage.

For related writes identify invariant and transaction owner. For external calls/files/mail describe completion semantics, post-commit work, duplicates and failure recovery. Do not present a database transaction as rollback for an external provider.

Connect conceptual entity relationships to permissions and lifecycle. Relate API resources/versions, route contracts, props/types and frontend forms to existing Boost/Rapide conventions rather than copying their syntax.

For each integration specify an entry/exit boundary and who owns the data; keep protocols out of unrelated controllers. Decide sync/async from user-visible waiting and durability requirements, not from a generic performance slogan.

## Lightweight decision record

Use DEC identifiers and the fields in [Decisions.md](../templates/Decisions.md). Include context/evidence, alternatives genuinely considered, choice, rationale, consequences and status/acceptance source.

Separate reversible defaults from expensive-to-reverse choices such as identity, ownership, retention and historical calculations. Record an acceptance or a resolution checkpoint before dependent work starts.

## Diagrams

Use Mermaid only if it clarifies a real ambiguity or makes a complex confirmed model easier to understand. Suitable diagrams include actors/external systems, domain dependencies, workflow/state transitions and entity cardinalities.

Use accepted terminology and labels, and show unknown boundaries explicitly. Do not invent entities, services, queues or external providers to fill a diagram. Check syntax and correspondence with prose; diagram validation limits must be disclosed if rendering is unavailable.

## Deviations and future considerations

Document a necessary departure from baseline as Proposed with the requirement, alternatives, costs and approving owner. Do not silently modify rules/dependencies. Keep optional future choices out of current MVP diagrams and Roadmap deliverables.

Validate architecture against [review-checklist.md](review-checklist.md): every technical element supports a requirement/control/confirmed constraint, and each accepted MVP flow has an achievable implementation path.
