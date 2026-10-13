# Roadmap

Authoring contract: generate docs/Roadmap.md after scope acceptance, using phases derived from this project's workflows and dependencies. Replace the instructions with actual slices; do not copy this authoring contract. No default authentication/billing/reporting phase is mandatory.

## Delivery Goal

State the accepted first usable business outcome, scope owner, Must/Should/Could/Out-of-scope boundary, constraints and known hard dates. Do not fabricate estimates or commitments.

## Foundation Status

Distinguish inherited capability, actual implemented/verified behavior and missing configuration/quality/operations work necessary for accepted requirements. Do not plan to rebuild existing foundation without a reason.

## Implementation Phases

Create a section per real phase with a meaningful number/name. For each include all of:

- **Goal:** usable outcome and related FR/BR/SEC.
- **Scope:** concrete included behavior and boundaries, not an entire vague domain.
- **Dependencies:** prior tasks, accepted DEC choices, external prerequisites and owners.
- **Deliverables:** small independently reviewable tasks, each with functional context, requirement IDs and completion result.
- **Acceptance criteria:** observable success, denial/invalid input, business invariants and important failure outcomes.
- **Tests:** appropriate automated regression, contract evidence, manual acceptance or operational verification; name what defect each catches.
- **Risks:** unknowns, assumptions, provider/data constraints and blocked work with resolution checkpoint.

Use a task table with Task, Functional result, FR/BR/SEC, Dependencies, Acceptance/verification and Status. Populate with actual tasks. State Proposed/Blocked where appropriate, not Complete for planned features.

Order phases by dependencies and usable slices. Include relevant security/quality enablers before exposing protected behavior. Split large workflows by coherent transitions/handoffs without leaving invariants unsafe.

## Requirement Coverage

Map Requirement/control ID, Priority, Phase/task, Acceptance criterion, Verification and Dependency/status. Every Must FR, applicable BR and required SEC must have a real owner/task and observable test/review outcome. Keep links to Product/Security records.

## Risks and Decision Checkpoints

List risks, open questions, assumptions and external dependencies with impact, owner, checkpoint and affected work. For an acknowledged critical unknown define a discovery task, exit decision and blocked dependents. State an infeasible MVP plainly if there is no safe path.

## Launch Acceptance

Define the agreed product acceptance, applicable quality gate, permission/security verification, data migration/reconciliation, operational readiness and recovery checks. Identify responsible owners and needed customer approval without claiming launch has occurred.

## Later Scope

Keep Should/Could improvements, long-term vision and unaccepted proposals separate. List exclusions explicitly. Do not promise future work simply because it was mentioned during discovery.
