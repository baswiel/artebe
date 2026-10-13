# Product, people and process question bank

Select only unanswered relevant prompts. Separate complex subjects into rounds; do not ask the entire bank. Use [discovery.md](discovery.md) for question limits and evidence status.

## Phase 1 — identity and outcomes

Establish project/public name, internal working name, client/sponsor, decision maker and a short plain-language description. Ask problem questions before stack questions:

- What should become possible that is difficult or impossible today? Who experiences that problem?
- How is the work done now? Request one concrete recent example, including manual steps and existing tools.
- Is this replacing a system, supplementing one or creating a new process?
- What primary business outcome justifies the project? Which secondary outcomes matter?
- What would make the sponsor call it successful in six months? Ask how the result is measured, against which baseline, by whom and when.
- What must the first usable release enable? What is explicitly outside scope?
- Which dates are hard commitments, what drives them and what scope can move?
- What belongs to the longer-term vision rather than the first release?

Resolve vague statements such as "automate everything" into a named process and measurable effect. Budget/team/capacity constraints belong in scope/roadmap discovery when they affect feasibility; do not invent a delivery estimate.

## Phase 2 — actors, roles and boundaries

Identify actual people and system actors: staff, customers, administrators, partners, guests, support, scheduled processes and API clients only if present.

For each actor establish who they are, why they use the system, frequent actions, permitted reads/writes, forbidden actions and the data boundary. Ask progressively:

- Describe a normal working day for this user; what should the application help them finish?
- Which records belong to them versus their team/company/location? Is any data shared?
- Can a person belong to more than one organisation or location? How do they switch context?
- Can users delegate work or permissions? Who can revoke that delegation?
- Are there platform operators who cross company boundaries? Under what approval/audit restrictions?
- Can anyone act as another user? If yes, follow the impersonation questions in [security.md](security.md).

Produce an actor/permission matrix: actor, goal, action, record scope, conditions, denied actions. Do not create a role package or assume a universal super-admin.

Explicitly classify the functional arrangement: one organisation, multiple isolated customer organisations, branches/teams within an organisation, intentionally shared data or cross-company management. A SaaS label is not enough to choose the tenancy implementation.

## Phase 3 — end-to-end workflows

Start with the most valuable workflow, then failures and secondary workflows. Request a real example from trigger to completion.

Capture each workflow with:

| Field | What to clarify |
| --- | --- |
| Trigger / actor | What starts it and who or which system initiates it? |
| Preconditions | What must already exist or be true? |
| Steps / handoffs | What happens in order, and who must act next? |
| Business rules | Which checks, limits, calculations and permissions apply? |
| Result | What changes, and how is successful completion recognized? |
| Notifications | Who is informed, through which channel and when? |
| Failure cases | Invalid input, timeout, missing actor, duplicate or concurrency |
| Exceptions | Who may override which rule, with what reason? |
| Reversal | What can be undone; what becomes permanent? |
| Audit | What must remain provable after the workflow? |

Follow up: what if the next actor does nothing, the same operation occurs twice, another user edits it concurrently, the deadline passes or an external response arrives late?

If states emerge, record a transition table: from state, event/action, permitted actor/scope, guards, resulting state, side effects, reversible or irreversible. Ask which transitions are forbidden. Draw a state diagram only after transitions are sufficiently confirmed; do not prescribe a state-machine package.

## Phase 4 — functional domains

Derive domains from actual responsibilities, terms and workflows; do not reuse generic Customers/Orders/Billing examples as project scope.

For each candidate establish responsibility, main entities/workflows, data owner, invariants and interactions with other responsibilities. Determine whether it is a domain, one feature within a domain or simply shared UI/supporting infrastructure.

Functional domains do not mandate new codebase modules. Resolve shared-data ownership and avoid two domains independently modifying the same invariant.

## Phase 6 — business rules and functional requirements

Ask what must never occur, what must always be true, invalid combinations, limits, calculation inputs/rounding and permitted exceptions. Check boundaries: zero, empty, maximum, expired, duplicate and conflicting concurrent actions when relevant.

Separate:

- Validation: accepted input shape/ranges and conditional fields.
- Authorization: who may act on which records under which context.
- Domain: allowed lifecycle, uniqueness, capacity and cross-record invariants.
- Calculation: formula, units, precision, rounding, effective dates and authoritative source.

Write each requirement with a stable FR identifier, actor, capability, conditions, observable result, priority and acceptance examples. Write each invariant as BR with a condition and required/forbidden outcome, including failure behavior.

An acceptance scenario must be falsifiable: given a specific state and actor, when the action occurs, then name the result and what must not change. "Users can manage maintenance" is insufficient; identify create/assign/reschedule/complete/cancel rights and outcomes separately if those actions exist.

## Phase 10 — search, only when needed

Ask what people search for and why, anticipated record count/growth, search fields, filters, sort options, matching expectations and acceptable index delay.

Clarify whether typo tolerance, full text, relevance ranking or simple structured filtering is required. Must old/archived records be searchable? Which permissions filter results, counts and suggestions?

Do not select a search engine until scale/matching behavior justify it. Link the proposed choice to [architecture.md](architecture.md).

## Phase 11 — notifications

For each real notification record trigger, recipient/scope, channel, timing, content/template owner, allowed delay, duplicate behavior, retry policy, preferences/opt-out and safe delivery/audit logging.

Ask which messages are necessary to finish the workflow versus optional reminders or communications. Determine whether a failed delivery blocks the business result and who handles terminal failure. Do not assume email, push, SMS or unsubscribe rules apply to every message.

## Phase 13 — reporting and exports

Ask which decisions a dashboard/report supports, audience, metric definitions, source data, filters/record scope, period, refresh delay and historical reproducibility.

For CSV/Excel/PDF clarify fields, sensitive-data access, size, async needs, scheduled recipients, export retention, safe file delivery and formula/content risks where relevant. Do not add a report just because a dashboard exists.

## Phases 16–17 — non-functional expectations and UX

Establish usable first-release screens and their primary actions, navigation, most frequent tasks, dashboard information, existing brand/design system and reusable component constraints.

Ask functionally about:

- Devices and context: desktop/mobile/tablet, field work, unreliable connectivity, PWA/native app expectations.
- Accessibility: keyboard/screen-reader users and required standard/testing expectations; do not assert certification without evidence.
- Localisation: languages, timezone ownership, daylight-saving behavior, currency, units, locale and date formats.
- Speed: which action needs which response time, estimated users/concurrency, records, imports and peak periods.
- Reliability: acceptable delays, duplicate actions, lost work and recovery expectations.
- Availability/growth: office hours versus continuous operation, maintenance windows and expected seasonal/growth patterns.

Distinguish estimates from contractual requirements. Define measurable acceptance conditions where evidence supports numbers; unknown volumes remain estimates/open questions rather than manufactured load targets.

Route operational implications to [operations.md](operations.md), protection to [security.md](security.md), and requirements to [roadmap.md](roadmap.md).
