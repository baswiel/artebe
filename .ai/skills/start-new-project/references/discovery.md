# Discovery protocol

Use this reference to run the interview and preserve state across rounds. Question banks are coverage aids: never send a whole reference as a questionnaire.

## Baseline and evidence registers

Record each baseline item with source path, observed version/setting, interpretation and verification limit. Classify it as:

| Classification | Meaning | Interview consequence |
| --- | --- | --- |
| Fixed by boilerplate | Explicit framework/team convention or existing foundation | Explain briefly; do not reopen without a requested or functionally necessary deviation |
| Project decision | A business-dependent choice despite available tooling | Ask about need, then propose a justified option |
| Still unknown | Insufficient evidence or conflicting source/config | Keep unresolved; ask a relevant owner rather than guessing |
| Not applicable | Excluded by confirmed scope | Record reason; revisit only when scope changes |

Keep a separate evidence status; classification and confirmation are different axes:

- **Confirmed:** explicit user statement or directly observed repository fact, with source. A repository fact confirms only what it shows.
- **Assumption:** unverified inference with reason, impact and a validation owner/checkpoint. Never silently promote it to Confirmed.
- **Proposed:** a suggested decision/option, with rationale and alternatives, awaiting user acceptance.
- **Open question:** unresolved information/choice, with impact, owner and next action.
- **Accepted decision:** explicitly accepted choice, with scope and source of acceptance. In Decisions.md use Accepted; replace old choices by Superseded rather than erasing them.

For example, an installed queue driver is Confirmed as a capability; a production worker remains Still unknown. Existing registration screens do not confirm that public registration belongs in the MVP.

Maintain conversational working registers for:

1. Baseline and sources; project identity and terminology.
2. Question history: topic, exact gap, answer/source, answered/partial/deferred/not applicable, follow-up reason.
3. Actors, ownership boundaries, workflows, entities and invariants.
4. Requirements/priorities and links to workflow, actor and business outcome.
5. Decisions, assumptions, risks, dependencies and open questions.
6. Phase applicability and coverage: covered, needs clarification, deferred or Not applicable with reason.

Do not create extra intake/ledger files. Use conversation summaries to retain state; the five final documents become the durable record after the gate.

## Round selection

- Start with identity/problem, users, current process, measurable outcome and MVP boundary. Include at most about five substantive questions initially.
- Use a short natural-language introduction, then numbered questions grouped around one concern. A question should request one answerable topic, not ten fields joined by commas.
- Choose the next group by uncertainty and impact, not by a rigid phase number.
- First establish the end-to-end core workflow; then explore its permissions, lifecycle, data and failure cases. Separate questions about each complex integration/workflow into their own rounds.
- Close relevant applicability checks without forcing full branches. A confirmed absence of integrations skips provider API/retry questions. A confirmed absence of uploads skips storage/scan questions.
- Do not ask technically fixed stack questions. Ask whether people must work without connectivity, for example, before proposing changes for offline behavior.
- Usually ask five grouped questions, at most ten substantive questions. Count subquestions independently when they introduce different decisions. For a single ambiguity ask one or two focused questions.
- After asking, end the response or use the host's supported input mechanism and wait for real user input. Do not continue into another interview round while answers are pending.

## Answer processing

Extract concrete nouns, actors, conditions, quantities and outcomes. Preserve the user's wording in the glossary until an accepted definition replaces it.

Before the next round check:

- Did the answer describe a goal or an actual requirement? "More efficient" needs a measurable outcome.
- Is "manage" specific enough to identify allowed reads, writes and lifecycle actions?
- Does "admin" mean within one company or platform-wide? Do not grant global access by name.
- Does "delete" mean hide, archive, erase or retain history? Who can do it?
- Does "real-time" mean seconds, minutes or simply no manual refresh? Ask for acceptable delay.
- Does "secure" mean a concrete access/data boundary? Ask for the harmful outcome to prevent.
- Are numbers estimates or contractual limits? Record provenance and confidence.
- Is a mentioned platform merely a current tool or a mandatory integration?
- Does one answer imply a new branch: shared companies, files, mobile/offline, external APIs, historical reconstruction or regulated data?

Summarize only changed/high-impact findings; do not repeat the full ledger each round. If the user answers later topics early, mark them covered and avoid asking them again.

## Vague answers and nontechnical users

Ask for one real example: actor, input, action, outcome and exceptional case. Offer 2–5 concrete examples when this helps, making clear the list is not exhaustive.

Translate technical questions:

| Avoid asking first | Ask functionally |
| --- | --- |
| Do you need multi-tenancy? | Can two customer companies ever see or change each other's work? |
| RBAC or ABAC? | Who may perform this action, on whose records, under which conditions? |
| Do you need idempotency? | What must happen if the same request arrives twice? |
| Redis or RabbitMQ? | May the user continue while this runs, and how long may completion take? |
| RPO/RTO? | How much recent work could be lost, and how long can the service be unavailable? |
| Event sourcing? | Must you reconstruct an old state, or is knowing who changed a field sufficient? |

If they do not know, explain options, recommend one and record it as Proposed. "Choose for me" authorizes a recommendation or decision only within the delegated scope; preserve its rationale and remaining uncertainty. An unrelated proposal still needs acceptance.

## Contradictions

State both statements neutrally, name the concrete consequence and ask which scope/condition resolves them. Distinguish an exception from a genuine conflict.

Example: "You said each company can see only its own records, and later that planners can move work between companies. Is that limited to companies in one group, or a platform-support permission?"

Never pick the most recent statement silently when it would alter ownership, irreversible actions, legal retention or MVP scope. Keep the dependent requirement Proposed/Open question until resolved.

When functional needs conflict with fixed rules, disclose the conflict, propose an approach within the rules and record a requested deviation for user decision. Do not edit the rules or recommend prohibited Livewire. Do not turn repository audit observations into new requirements.

## Unknowns, risks and dependencies

Use a compact table: ID, type, topic, evidence, impact, owner, resolution checkpoint, dependent requirements and status. Use identifiers such as OQ-001, ASM-001 and RISK-001 when cross-links help.

Critical gaps include core actor/workflow/MVP uncertainty, unclear protected-record ownership, unresolved dangerous transitions, sensitive-data requirements that determine design and essential integration feasibility.

An acknowledged unknown can remain only with an explicit boundary: what can be planned safely, what is blocked, who resolves it and before which milestone. If no safe boundary exists, continue interviewing; do not describe a blocked MVP as executable.

## Continuation and changes

On resume, read conversation summaries and existing project docs; show only uncertain/stale items. Do not start from round one again. Track new requirements against the already confirmed scope and decisions.

When the user changes a requirement, update all affected roles, workflows, security controls, data lifecycle and roadmap references. Explain material consequences and confirm the changed scope before generation. Never invent new IDs for unchanged requirements or reuse retired IDs for different behavior.

## Before documentation

Use [review-checklist.md](review-checklist.md). Present the compact evidence-status summary and ask for confirmation of that scope if not already explicit. Confirmation must cover the actual proposed content, not just "continue asking questions". Do not write docs while waiting.
