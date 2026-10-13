# Final review and document reconciliation

Run the pre-generation gate before writing files, then this checklist again on all five outputs. A filled heading is not evidence of a resolved requirement.

## Pre-generation gate

Present a compact user-facing summary of Confirmed facts, Accepted decisions, Assumptions, Open questions and Out of scope. Include project purpose, primary actors, main workflow, record ownership, MVP, major constraints and risks.

Check:

- [ ] Problem/current process and primary business goal are concrete; success criteria are measurable or explicitly owned unknowns.
- [ ] Primary users, roles and user/company/team record boundaries are known.
- [ ] Core workflows include trigger, actor, preconditions, steps, rules, result, failures, reversal and relevant audit.
- [ ] State transitions, concurrency/duplicate semantics and meaningful exceptions are explicit where applicable.
- [ ] Core entities, relationships, data ownership and lifecycle/history requirements are understood.
- [ ] Requirements and business rules are testable; priorities/MVP and exclusions are accepted.
- [ ] Authentication journeys and server-side authorization are clear enough for design.
- [ ] Actual integrations have purpose, source ownership, direction, failure semantics and contract/feasibility status; absent integrations are marked Not applicable.
- [ ] Upload/search/notification/reporting/API questions are answered or Not applicable for a confirmed reason.
- [ ] Sensitive data, access, retention/export/deletion, threats and controls are covered; legal/privacy validation has an owner where needed.
- [ ] Relevant non-functional, UX/device/localisation, operational and migration constraints are recorded.
- [ ] Important architecture choices have requirement-based rationale and acceptance status, consistent with `.ai/rules` and AGENTS.md.
- [ ] Risks/dependencies/assumptions/open questions have impact, owner and checkpoint. Deferred critical choices are explicitly bounded and do not make the claimed MVP infeasible.
- [ ] The user explicitly confirmed this scope summary, or equivalent exact scope earlier in the session. Silence, "continue" during interview and partial approval do not imply blanket acceptance.

If a critical item fails, ask targeted follow-ups and wait. If a user accepts a bounded unresolved item, document that acknowledgment and block the dependent work; do not substitute a guessed answer.

## Generation requirements

- [ ] Re-read relevant rules and existing target documents before editing.
- [ ] Generate exactly the five requested project documents under repository-root docs, with required template sections.
- [ ] Replace template guidance with actual intake content, without scaffold text, fake sample IDs or invented facts.
- [ ] Justify Not applicable sections; identify owned questions where information remains unknown.
- [ ] Keep current confirmed behavior, proposed design and implemented/verified state distinct.
- [ ] Preserve unrelated existing content and stable IDs; do not silently overwrite unresolved conflicting edits.

## Cross-document review

| Check | Reconcile |
| --- | --- |
| Terms | Same glossary names for roles, domains, entities, states and integrations |
| Scope | Product's MVP matches Roadmap deliverables; exclusions do not reappear as tasks |
| Access | Product Permissions, Security Authorization and Architecture scopes agree |
| Data | Cardinality, uniqueness, timezones, ownership, retention and deletion agree |
| Requirements | Every Must FR/BR maps to a roadmap slice and observable acceptance/test |
| Controls | Every required SEC has an architecture treatment and roadmap/verification owner |
| Technology | Every technical element has an FR/BR/SEC/DEC or confirmed constraint reason |
| Decisions | Accepted/Proposed/Superseded are consistent across text and diagrams |
| Dependencies | Tasks do not precede prerequisites or rely on unverified essential services |
| Unknowns | Same uncertainty, owner and blocking effect wherever referenced |
| Links/IDs | Local links resolve, IDs are unique/stable and referenced records exist |
| Diagrams | Valid Mermaid intent/syntax, real actors/entities, consistent transitions/cardinalities |
| Launch | Relevant quality, security, operations/migration acceptance is planned, not claimed complete |

Repair editorial differences, broken links and missing mappings using confirmed evidence. If fixing a mismatch requires a new product/permission/retention choice, return to the user; a consistency pass is not authorization to choose silently.

Read every document again after repair. Report an unavailable Mermaid renderer or other unexecuted verification accurately.

## Completion gate

Finish when scope is confirmed; critical questions are resolved or explicitly bounded; five documents exist and agree; MVP tasks have executable dependencies and acceptance; important decisions have rationale.

Link outputs and report remaining owner-bound questions, blocked tasks and verification limits. Supply the project-required Conventional Commit suggestions. Do not begin feature/foundation implementation without a separately requested follow-up.

## Skill maintenance dry-run

When validating this skill, use a separate temporary workspace and synthetic responses. Do not generate example Product/Architecture documents into the real boilerplate or confuse simulated confirmation with a real project intake.

Start with: "Start een nieuw project. We willen een SaaS-platform waarmee bedrijven hun onderhoudswerkzaamheden kunnen plannen."

Evaluate observable behavior over several turns:

- The first response inspects baseline, asks product questions and waits, with no document writes or assumed billing/roles/providers.
- Concrete responses lead to workflow/actor/ownership follow-ups instead of repeated questions or premature schema design.
- Vague roles, changing ownership and contradictory sharing statements trigger focused clarification.
- Confirmed absence of integrations/uploads/search requirements prunes the detailed branches; later changes reopen only affected branches.
- Unknown technical choices become explained proposals, not silently Accepted decisions.
- Confirmation is required for actual final scope; a bounded unknown remains visible and blocks dependent work.
- Only after that gate are five consistent documents produced; no application/dependency changes occur.

This is a scenario for behavioral evaluation, not a default product or questionnaire to copy into customer projects.
