# Scope, priorities and executable roadmap

Use phases 21–22 after enough product evidence exists; revisit when scope or dependencies change. Use [Roadmap.md](../templates/Roadmap.md) only after final review.

## Prioritization interview

Ask what must work to deliver the first business outcome, what can remain manual, what may follow later and what is explicitly excluded. Confirm priorities with the scope owner:

| Priority | Meaning |
| --- | --- |
| Must have | Necessary for the agreed MVP outcome or essential safety/launch condition |
| Should have | Important but first release can deliver the outcome without it |
| Could have | Optional improvement after higher priorities |
| Out of scope | Explicit exclusion; not an implied backlog commitment |

Separate long-term vision from a committed later phase. Ask who can decide tradeoffs, team/capacity/budget constraints and hard dates when relevant. Do not invent estimates or guarantee a date with unknown capacity.

If everything is Must, ask what could be done manually or omitted while still reaching the accepted success criterion. Explain essential security/privacy/operations constraints: they are not optional merely because they are less visible than a screen.

## Slice by outcome

Build real project phases from dependencies and usable results, not a generic Foundation/Auth/Billing sequence. Existing authentication may need configuration/hardening rather than recreation.

Each phase states goal, scope, dependencies, deliverables, acceptance criteria, tests and risks. Each task states user/business context, requirement/control IDs, concrete result, dependencies and observable completion.

Prefer independently reviewable vertical slices: one actor can complete one workflow under defined conditions with permission/failure coverage. Split a large workflow into coherent transitions or handoffs without fragmenting an invariant into unsafe partial behavior.

Avoid "build backend", "do frontend" or "set up services" without functional result. Include small foundation/enabler tasks only where an accepted requirement depends on them, such as activating a worker needed for delayed processing.

Do not assume packages are installed, integrations work, tests pass or deployment exists. Identify discovery/configuration/implementation/verification separately where necessary.

## Traceability

Assign stable IDs to requirements, business rules, controls and decisions; retain them on updates. Use task IDs such as TASK-001 only when useful; do not replace requirement IDs with phase numbers.

Maintain a mapping with:

| Requirement/control | Priority | Phase/task | Acceptance | Verification | Dependency / status |
| --- | --- | --- | --- | --- | --- |

Populate it with actual IDs/content, not illustrative rows. Map every Must requirement, relevant BR and required SEC control to an implementation slice and test/review evidence. A control shared by several tasks can have one owning task and explicit dependents.

Make authorization/validation/business/failure testing visible without creating a separate test task for every framework method. Distinguish manual/customer acceptance, automated regression, contract verification and operational checks.

## Risks and decision checkpoints

Record risks, assumptions, unknowns, external dependencies and decisions needed, with owner, impact and resolution checkpoint. Ask the user to confirm/add to this list before final review.

Put integration feasibility, unresolved ownership/retention and important capacity questions before dependent implementation. A time-bounded discovery task needs a concrete question and exit decision, not an open-ended research phase.

For a deferred critical decision mark affected tasks blocked and retain an executable safe path. If no viable MVP path remains, state this and keep the interview open rather than calling the roadmap complete.

## Launch and later work

Include relevant quality, security, data migration, backup/recovery, observability and user acceptance in launch criteria. Separate deployment plan from permission to deploy.

Keep Should/Could work and unaccepted alternatives outside MVP acceptance. Future considerations are possibilities until scoped, prioritized and accepted. Never insert billing, subscriptions, a search engine or services purely from a SaaS label.
