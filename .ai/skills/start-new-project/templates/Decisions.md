# Decisions

Authoring contract: generate docs/Decisions.md after the review gate. Replace instructions with evidence-backed decisions and open decision records. This is a lightweight ADR log, not a list of every routine Laravel convention; do not copy the authoring contract into output.

## Baseline and Status

Summarize what is Fixed by boilerplate, a Project decision, Still unknown or Not applicable, with evidence paths and verification limits. Keep Confirmed facts, Assumptions, Proposed choices and Accepted decisions separate.

Use these decision statuses:

- **Proposed:** awaiting acceptance; not a roadmap commitment.
- **Accepted:** explicitly chosen within a stated scope; record acceptance source.
- **Superseded:** replaced by a named later decision; retain original rationale/history.

## Decision Records

Create one heading per actual important decision: stable DEC identifier and its real title. Number from DEC-001, preserving existing IDs on updates. Include these fields in every record:

- **Status:** Proposed, Accepted or Superseded.
- **Context:** confirmed need, inherited constraint, evidence and unresolved assumptions.
- **Decision:** actual choice and scope, or a clearly Proposed choice; no implied acceptance.
- **Alternatives:** credible options considered, including retaining the baseline when applicable.
- **Rationale:** why this choice meets the requirement better in this context.
- **Consequences:** functional/technical tradeoffs, operational cost, reversibility and dependencies.
- **Acceptance source:** user confirmation or existing explicit agreement; if Proposed, name the decision owner/checkpoint instead.
- **Traceability:** relevant FR/BR/SEC, architecture section and roadmap task/phase.
- **Supersession:** replaced/replacing DEC IDs when applicable.

Record expensive-to-reverse boundaries such as ownership/tenancy, identity, historical meaning, retention and essential integration semantics. Do not inflate trivial syntax choices into ADRs.

## Assumptions

List genuine Assumption records with evidence/reason, impact, validation owner/checkpoint and affected decisions/requirements. Do not represent an unaccepted recommendation as a fact.

## Decisions Needed

Record Open questions and Proposed choices still requiring a decision, with options, recommendation/rationale where possible, impact, owner, checkpoint and blocked work. Explicitly identify accepted deferral boundaries and what remains safely executable.
