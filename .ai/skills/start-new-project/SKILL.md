---
name: start-new-project
description: "Interview the user in multiple rounds to define a new Laravel project based on the Rapide boilerplate, then produce Product, Architecture, Roadmap, Security and Decisions documents after scope confirmation. Use for project kickoff, new-project intake or an explicit start-new-project invocation; not for implementing features, cloning a repository or a one-off technical question."
---

# Start New Project

Understand the problem, people, processes and constraints before selecting architecture or writing project documents. This skill conducts discovery; it does not implement the application.

## Contract

- Communicate in the user's language and explain technical implications in functional terms.
- Inspect the project before the first question. Treat observed configuration as evidence, not a confirmed product requirement or proof of deployed infrastructure.
- Ask successive rounds of about 5 questions, never more than 10 substantive questions per round. Use fewer for a focused clarification or final confirmation; do not hide a long questionnaire inside subquestions.
- Wait for the user's answer after each round. Do not simulate answers, complete the interview alone or treat elapsed time/silence as acceptance.
- Skip answered or irrelevant questions; follow up only on a missing detail, conflict or changed requirement.
- Keep facts, assumptions, proposals, decisions and unresolved questions distinct. A recommendation is Proposed until explicitly accepted.
- Do not write any of the five project documents during discovery. Keep working summaries in the conversation until the review gate passes.
- Read the applicable `.ai/rules` again before generation. Follow AGENTS.md/Boost for framework conventions; do not duplicate their implementation manuals.
- Do not implement features, migrations, models/controllers, install packages, change dependencies, edit rules/AGENTS.md, configure hosting or deploy. A requested implementation is a separate follow-up after discovery.
- Do not collect passwords, tokens or real customer records. Ask for classifications, sanitized examples and responsible owners.

## Phase 0: inspect before interviewing

Resolve the repository root from the workspace; this skill's directory is not the project root.

1. Read root and applicable nested AGENTS.md files, `.ai/rules/index.md`, and its architecture, backend, database, frontend, testing, security, quality and git-workflow rules when present. Search rules for the project concerns. Missing instructions remain a disclosed gap, not invented conventions.
2. Inspect composer.json/package.json and lockfiles, relevant config and bootstrap, routes, frontend entrypoints/pages/layouts, models/migrations, tests/phpunit.xml and CI/deployment files. Read existing project docs and establish whether this is a fresh intake or a continuation.
3. Follow Boost's version/tool instructions for API-dependent choices. If dependencies or tools are unavailable, use manifests/lockfiles for evidence and label runtime behavior unverified. Do not install dependencies to conduct intake or read secret environment values.
4. Build the baseline register described in [discovery.md](references/discovery.md). Distinguish installed capability, declared configuration, implemented behavior and verified deployment. Detect manifest/runtime/CI conflicts without silently choosing a winner.
5. Briefly state the observed fixed baseline and material gaps, then begin with the product problem. Do not ask the user to choose Vue versus React or a test framework already fixed by this boilerplate. Registration, required 2FA, tenancy, database hosting and active workers can still be project decisions.

The current Rapide rules exclude Livewire. Respect that choice; do not offer it as an option. Re-inspect versions and project configuration at every new intake instead of freezing this template's current values into a new project's definition.

## Interview loop

Read [discovery.md](references/discovery.md) first; it defines the evidence ledger, round selection, conflict handling and resume behavior. Load other references only for the current concern.

After every answered round:

1. Extract confirmed statements and update the ledger, terminology, priorities and applicability map.
2. Summarize important additions/corrections in a few lines. Mark each unconfirmed inference explicitly.
3. Detect vague answers, contradictions, unsafe ownership boundaries and unresolved dependencies. Explain the consequence and ask a focused functional follow-up; give 2–5 examples/options if useful.
4. Select the next unanswered high-impact group. Explain technical consequences only when relevant to that functional choice; delay architecture selection until product/domain discovery is sufficient.
5. Ask the next round and yield to the user. Work must remain paused on unanswered critical decisions.

Use all applicable phases; phases are coverage checkpoints, not 24 mandatory questionnaire rounds. Combine related checkpoints and use multiple rounds for a complex workflow or integration.

| Phases | Concern | Read when relevant |
| --- | --- | --- |
| 1 | Identity, problem, outcomes, constraints | [product.md](references/product.md) |
| 2–4 | People/roles, workflows, functional domains | [product.md](references/product.md) |
| 5–6 | Entities, lifecycle, history, testable invariants | [data-model.md](references/data-model.md), [product.md](references/product.md) |
| 7 | Login, accounts and record-level access | [security.md](references/security.md) |
| 8–9 | External systems and files/uploads | [integrations.md](references/integrations.md), [security.md](references/security.md) |
| 10–11 | Search and notifications | [product.md](references/product.md) |
| 12–14 | Background processing, reporting, audit/history | [operations.md](references/operations.md), [data-model.md](references/data-model.md) |
| 15 | Privacy, threats and controls | [security.md](references/security.md) |
| 16–17 | Non-functional requirements and UX | [product.md](references/product.md), [operations.md](references/operations.md) |
| 18–20 | Consumed/exposed APIs, deployment and migration | [integrations.md](references/integrations.md), [operations.md](references/operations.md) |
| 21–22 | MVP, priorities, risks and unknowns | [roadmap.md](references/roadmap.md), [discovery.md](references/discovery.md) |
| 23 | Justified architecture decisions | [architecture.md](references/architecture.md) |
| 24 | Confirm scope and readiness | [review-checklist.md](references/review-checklist.md) |

Do not infer billing, subscription plans, integrations, tenant isolation technology or an administrator's permissions from the word SaaS. Investigate the functional boundaries first.

## Final review gate

Use [review-checklist.md](references/review-checklist.md) before generating documents.

- Present a compact summary of Confirmed facts, Accepted decisions, Assumptions, Open questions and Out of scope, including the MVP and highest-impact risks.
- Resolve missing critical information with additional rounds. An unknown may remain only if the user explicitly accepts its impact, owner and resolution checkpoint; mark affected work as blocked and identify a safe planning boundary.
- Ask for scope confirmation if not already given for this exact summary. Reuse explicit confirmation from the session; do not ask again without a material change. Partial acceptance is not acceptance of every proposal.
- Do not generate until core scope is confirmed and critical questions are answered or explicitly bounded. The instruction to start an intake alone is not scope confirmation.

## Generate and reconcile

After the gate, create/update only `docs/Product.md`, `docs/Architecture.md`, `docs/Roadmap.md`, `docs/Security.md` and `docs/Decisions.md` at the repository root.

Read these authoring templates; they contain writing instructions, not sample project facts:

- [Product.md](templates/Product.md): comprehensive functional definition.
- [Architecture.md](templates/Architecture.md): requirement-driven technical design.
- [Roadmap.md](templates/Roadmap.md): small implementation slices with acceptance/tests.
- [Security.md](templates/Security.md): access, data, threats and required controls.
- [Decisions.md](templates/Decisions.md): lightweight ADRs and remaining decisions.

Replace authoring guidance with the confirmed intake content; do not copy instructions or empty scaffolding into the documents. Preserve the required sections, using justified Not applicable or an owned Open question rather than inventing content. Detail follows project complexity, not a page target.

Inspect existing target documents before editing. Preserve confirmed content and unrelated edits; reconcile incremental changes instead of replacing documents wholesale. Ask only when an unresolved conflict or ambiguous overwrite would lose user work. Re-run the review gate if new facts materially alter the confirmed scope.

Use stable FR/BR/SEC/DEC identifiers and real roadmap phases for traceability. Check every MVP requirement and security control against its planned deliverable, acceptance criteria and test; proposals are not implementation commitments. Add Mermaid only when it explains a real context, relationship or workflow.

Re-read all five outputs with [review-checklist.md](references/review-checklist.md). Fix terminology, links, identifiers and missing coverage automatically from confirmed evidence. Return to the user for a substantive product/permission/retention conflict; do not resolve it by assumption.

## Completion

Finish only when scope is confirmed, critical questions are resolved or explicitly bounded, all five documents exist, their content is consistent, the MVP roadmap is executable and important architecture choices have reasons.

Link the five outputs, report unresolved questions/blocked work and verification limits, and provide the commit suggestions required by the project. Do not imply any planned feature, security control or operational service is already implemented. Foundation implementation requires a separately authorized follow-up.
