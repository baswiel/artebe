# Entities, lifecycle and historical meaning

Use after workflows/actors are understood (phases 5–6), and again for audit/history and migration (14/20). Design concepts before proposing tables. Follow the project database rules for implementation conventions.

## Entity card

For each actual entity establish:

| Concern | Capture |
| --- | --- |
| Meaning | User's term, responsibility and concrete example |
| Identity | Business identifier, uniqueness scope, source and reuse rules |
| Properties | Necessary attributes, units, required/optional meaning and authoritative source |
| Ownership | User/company/team/location scope; who can move ownership |
| Relationships | Cardinality, optionality, constraint and lifecycle behavior |
| Lifecycle | Creation, states, allowed transitions, finality and deletion |
| History | Changes to retain, who/when/why and effective period |
| Privacy | Data classification, permitted access and minimization |
| Retention | Business/legal requirement, owner, deletion/export policy and open validation |

Do not automatically create a database table for every noun. A value, calculated view or integration payload may not need independent persistence.

## Relationship questions

- Can each side have one or many of the other, now and over time?
- Can the relationship be temporarily missing? What does absence mean?
- Is the link itself meaningful (assignment date, role, price, duration)?
- What happens if either side is deactivated, archived or erased?
- Can related records belong to different companies? If so, who authorizes that sharing?
- Which identity remains stable across imports/provider changes?

Record confirmed one-to-one, one-to-many, many-to-many, optional and historical links. Preserve uniqueness scope and ownership in Architecture and Security; do not infer shared-database or separate-database tenancy yet.

## Time-dependent data

Investigate scheduling, prices, assignments and policy values when the workflow depends on dates:

- Does a time represent a date, a local appointment time, an instant or a validity period?
- Whose timezone applies, including daylight-saving and cross-zone users?
- Are endpoints inclusive? Can periods overlap or leave gaps? Are open-ended periods allowed?
- Must a later report reproduce the value that applied on a past date?
- Can a backdated correction change a historical result? Who may make it, and what must remain visible?
- Are future changes scheduled, and do existing records use a snapshot or current value?

Names such as start_date/end_date, valid_from/valid_until and effective_at are possible concepts, not mandated columns. Distinguish event occurrence, record creation and business-effective time. Add historical periods/snapshots only for a confirmed reconstruction requirement.

## Audit versus history

Ask whether the goal is troubleshooting, knowing who changed a value, seeing old versions or proving an immutable event. Distinguish application logs, audit trail, model history and immutable business records.

Capture actions/fields recorded, actor including service/impersonation, old/new values or event meaning, reason, timestamp, reader rights, retention and tamper expectations. Do not assume an audit package or event sourcing.

Check whether deleting personal data conflicts with historical proof or backups. Record the concrete conflict and legal/privacy validation owner; use [security.md](security.md), not an invented universal retention period.

## From invariants to persistence

Only after confirmation propose a conceptual data model: entity, relationship, uniqueness, state and lifecycle. Use an ER diagram when it clarifies confirmed cardinalities; visibly label uncertain relationships or omit them until resolved.

Identify which invariants need database constraints, atomic updates/locks or a transaction across writes. Consider duplicate requests, concurrent assignments and partial failure. Do not write SQL schemas or migrations during intake.

For existing data, route source/mapping/cutover questions to [operations.md](operations.md). Link each important model choice to its FR/BR and DEC records; do not assert all illustrative entities are accepted scope.
