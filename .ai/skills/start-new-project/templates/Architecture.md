# Architecture

Authoring contract: generate docs/Architecture.md only after scope review. Replace guidance with requirement-backed design, observed baseline and explicitly labeled proposals/unknowns. Retain these sections; justify Not applicable. Link Product/Security/Decisions/Roadmap using paths relative to the final docs directory. Do not copy this authoring contract into output.

## Architecture Overview

Explain how the accepted product operates as a system. Separate inherited boilerplate, proposed/accepted project choices and verified implementation/deployment state. State important evidence/verification limits.

## Architectural Principles

Reference the applicable AGENTS.md and Rapide rules. Explain only project-specific consequences; do not restate Laravel manuals. Preserve Vue/Inertia and the exclusion of Livewire. Every extra abstraction must serve a concrete requirement.

## System Context

Describe actual actors, external systems, trust boundaries and data flows. Add a Mermaid context diagram only if useful and based on confirmed scope; label uncertain boundaries.

## Application Boundaries

Identify Laravel app responsibilities and external/provider ownership. Justify any separate service with requirement/DEC evidence; avoid speculative microservices.

## Domain Structure

Connect functional domains to existing Laravel responsibilities/folders without imposing a new module tree. State shared-invariant owners and reasons for any proposed Actions/Services/adapters/DTOs/Queries/Events/Jobs.

## Request Flow

Describe representative confirmed workflows from input through authorization/validation, business operation, persistence and response. State failure and side-effect completion semantics; reference FR/BR.

## Authentication

Describe accepted account lifecycle and required authentication behavior against the existing foundation. Distinguish enabled capability from product requirement, and accepted SSO/2FA/API needs from proposals.

## Authorization

Map actor/action/ownership/organisation/team conditions to the server-side enforcement design. Keep tenancy scope and cross-company/support exceptions aligned with Product and Security.

## Data Model

Present conceptual entities, cardinalities, optionality, uniqueness, ownership, states and historical/effective-time meaning. Use ER diagrams only for useful confirmed relationships. No generated migrations/schema code.

## Database Strategy

State accepted engine/hosting constraints or the decision still needed, invariant enforcement, relevant indexes/query concerns and business transaction ownership. Explain any test/production-engine difference without assuming a deployment default.

## Background Processing

Identify async work, acceptable delay, progress/completion/failure results, retry/duplicate semantics, commit dependencies and required worker readiness. Explain why background work is necessary.

## Scheduler

Describe confirmed timed work, cadence/timezone, missed runs, overlap/concurrency requirements and operational owner. Avoid claiming cron is running from a manifest alone.

## Integrations

For each boundary state purpose, contract evidence/version, mapping/source of truth, direction, credentials owner, timeouts/quotas, idempotency/retries, failure/reconciliation and monitoring. Record unverified feasibility.

## APIs

Describe actual consumers, resource/version/error contracts, identity/record scope, filters/pagination/rate limits and documentation needs according to existing rules. Mark Not applicable when no separately consumed API exists.

## Files and Storage

Record sensitivity/access, volume/types/limits, storage constraints, processing/scan policy, delivery/download rights, deletion/retention and readiness owner for actual uploads/exports.

## Search

Relate matching, data size, filters, permission scoping and freshness to the simplest suitable solution. Link an engine/index decision to DEC and its operational consequences.

## Notifications

Describe actual channels/trigger/recipient boundaries, content ownership, preference rules, retry/duplicate/failure handling and delivery observability.

## Logging

Define diagnostic purposes, safe correlation fields, readers, redaction, retention and error reporting. Distinguish existing wiring from verified operations.

## Audit

State what business changes/actions must be reconstructable, actor/old-new/reason/time needs, permitted readers, integrity and retention. Separate logs, model history and immutable records.

## Caching

Justify cached data using performance/staleness requirements; define scope, TTL/invalidation and permission/tenant boundaries. Mark unnecessary caching Not applicable rather than inventing Redis.

## Security

Link required SEC controls to trust boundaries and architecture treatments. Keep personal data/retention, ownership, secrets, upload/API protection and planned verification aligned with Security.

## Performance

Connect accepted response/load targets to bounded queries, processing choices and representative verification. Treat unknown capacity as owned discovery, not a guessed benchmark.

## Scalability

Describe expected growth/peaks and justified capacity options. Separate current needs from future hypotheses; no speculative cluster/services as MVP requirements.

## Error Handling

Distinguish invalid input, denied access, domain failure, transport/provider failure and unexpected errors. Describe user-visible results, safe diagnostics, retry versus terminal handling and consistency/recovery.

## Observability

Connect important failures and service expectations to metrics/logs/alerts, recipients, response and verification. Distinguish application health from external dependency health.

## Deployment

Record inherited workflow, accepted environments/hosting constraints, configuration/secrets ownership, migrations, worker/scheduler readiness, release/recovery, backups and restoration checks. Do not configure or deploy during intake.

## Testing Strategy

Plan requirement/authorization/validation/business/failure regression, isolated database fixtures, external fakes/opt-in contracts, engine-specific verification and relevant operational/customer acceptance. Refer to project gates rather than duplicating framework instructions.

## Technical Risks

List requirement-dependent technical uncertainty, assumptions, integration/operational gaps and blockers with owners/checkpoints and affected roadmap work.

## Future Considerations

Record deferred possibilities with triggers for reconsideration. Keep unaccepted technology and out-of-scope features separate from MVP architecture.
