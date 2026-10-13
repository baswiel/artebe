# Processing, service expectations, deployment and migration

Read for phases 12–14, 16 and 19–20. Start with repository evidence, then ask only project-specific operational questions. Installed/configured infrastructure is not verified deployment.

## Background processing and scheduling

Identify actual imports, exports, synchronization, calculations, PDF/media processing, mail and timed actions. Ask:

- Must the user wait for completion, or can they continue and see progress/result later?
- How long may processing take and how is completion or failure communicated?
- Which steps need durable retries, and which effects must occur only once despite repeated execution?
- Can the same record/process be handled concurrently? What must happen if it is already running?
- Which tasks run at a deadline/frequency, in whose timezone, including weekends and daylight-saving changes?
- What should happen after downtime: catch up, skip missed runs or ask for intervention?
- Who investigates terminal failure, and can they safely replay or cancel work?

Only then recommend synchronous work, a job or a schedule. Record worker/cron activation and monitoring as required deliverables when async/timed behavior is essential. Confirm reservation/timeouts and after-commit semantics through the existing rules/documentation when planning technical detail.

Do not mandate Redis/Horizon/RabbitMQ. A configured driver might satisfy the need, but needs actual workers, persistence/failure handling and deployment ownership. An installed RabbitMQ package is not evidence of a running broker.

## Reporting and history consequences

Use [product.md](product.md) for report purpose and [data-model.md](data-model.md) for audit/historical reconstruction. Establish processing lag, allowed data freshness, safe exports, reproducible periods/snapshots and delivery failure behavior only for actual reports.

## Non-functional requirements

Record estimates separately from accepted targets. Ask about user/concurrency counts, dataset sizes, growth, peaks and bulk workloads only enough to justify design/testing.

Define important response-time targets around an actual action and representative load. "Fast" needs a measurable or explicitly unresolved expectation. Do not promise extreme scale without evidence.

Establish operating hours, availability expectations, maintenance windows and acceptable degraded behavior. Reliability includes duplicate processing, incomplete external steps and losing recent work, not only uptime.

When recovery matters, translate RPO/RTO into tolerable data loss and downtime. Ask who approves those targets, backup coverage/frequency/retention/location, restoration responsibilities and how restoration is tested. Configuration alone is not proof of recoverability.

## Deployment and service ownership

Inspect CI/container/deployment config, environments, health endpoints, logging and existing monitoring. Distinguish inherited workflow from decisions still requiring a host/operator/customer owner.

Ask relevant project-specific questions about:

- Development/test/staging/production environments and who may access or approve releases.
- Hosting/data-location constraints and owners; retain inherited Docker/Kubernetes/CI conventions where applicable.
- Needed workers, cron, object storage, cache and provider connectivity; lifecycle/restarts during releases.
- Secrets/config provisioning and separation, without requesting values.
- Error tracking, log aggregation, safe diagnostic context, alert recipients and response expectations.
- Health/readiness behavior and which critical dependencies need separate monitoring.
- Database migration/compatibility, release recovery and backup/restoration.

No hosting configuration, deployment or account creation occurs during this skill. Record operational deliverables in Roadmap rather than asserting they are live.

If the current CI only tests Unit while rules require Feature plus other checks, record the observed gap and the task needed for launch. Do not silently repair CI or describe the planned gate as passed.

## Existing-system migration — only if replacing/importing data

Obtain source owner/access, format, volume, quality, duplicates, identifiers, mappings, relationships and sensitivity. Ask explicitly whether all history or only current records must move.

Clarify data cleansing responsibility, matching/unmapped-record handling, timezone/units, incremental changes during migration and reconciliation criteria. Ask who signs off on counts, balances or representative samples when those are relevant.

Define cutover: when writes stop or synchronization switches, user communication, test rehearsal, acceptance, rollback boundary and old-system access/decommissioning/retention. A destructive step needs explicit ownership and later implementation approval, not authorization inferred from discovery.

Distinguish rollback of code from restoration/reconciliation of newly written business data. Do not claim a backup alone can safely undo external transactions.

## Handoff

Connect accepted expectations to FR/SEC/DEC IDs and concrete roadmap verification. Route unresolved operational requirements to an owned checkpoint; keep essential launch dependencies out of an unqualified "future improvements" list.
