# Product

Authoring contract: use the headings below when generating docs/Product.md after the scope gate. Replace these instructions with confirmed project content. Use the user's language for prose; retain stable identifiers and shared terms. Explain Not applicable or owned Open questions instead of leaving empty sections. Do not copy this authoring contract into the output.

## Executive Summary

State project/client, primary users, problem, first-release value and scope in plain language. Distinguish accepted MVP from future vision; identify scope owner and confirmation source/date when known.

## Problem Statement

Describe current software/manual process, affected people, concrete bottlenecks and consequences. Include actual examples; distinguish observed facts from user estimates.

## Goals

Record primary and secondary business outcomes with priority and owner. Separate outcomes from implementation features.

## Non-Goals

List explicit exclusions and their rationale/boundary. Do not imply excluded work is committed for a later release.

## Success Criteria

For each criterion identify metric/outcome, baseline, target, measurement source, review horizon and owner. Mark missing targets as owned Open questions rather than inventing numbers.

## Stakeholders

Identify client/sponsor, product/scope decision maker, domain experts, users and relevant security/privacy/operations/provider owners. State who accepts which deliverable; do not invent names.

## Users and Roles

Describe every actual human/system actor, purpose, frequent actions, hierarchy/membership and organisational boundary. Identify guests/API clients only if in scope.

## User Needs

Connect each user need to the problem and required outcome, with frequency, context, devices and constraints where relevant.

## Domains

Describe confirmed functional responsibilities, entities, workflows, data owners and dependencies. Distinguish a domain from a supporting feature; this is not a mandated directory tree.

## Core Entities

Define entities functionally with identity/uniqueness scope, important properties, ownership, relationships and lifecycle. Link to Architecture's conceptual model; do not include unaccepted illustrative entities.

## Functional Requirements

Use stable FR identifiers. For each requirement state actor, capability, conditions, observable outcome, priority, related workflow/BR/SEC and acceptance examples. Split vague management features into actual actions. Distinguish Confirmed requirements from Proposed features.

## Core Workflows

For each workflow capture Trigger, Actor, Preconditions, Steps, Business rules, Result, Notifications, Failure cases, Exceptions and Audit requirements. Include handoffs, duplicate/concurrent actions, reversal and applicable state-transition tables. Link requirements and rules.

## Business Rules

Use BR identifiers. Distinguish validation, authorization, domain and calculation rules. Express conditions, invariant/outcome, failure behavior, boundary examples, exceptions and owner. Preserve units, precision, effective dates and formula provenance where relevant.

## Permissions

Provide actor/action/record-scope/conditions/denied-action matrix. Explain cross-user/company/team access, delegation and support/impersonation boundaries where applicable. Align with Security; no invented universal admin rights.

## Notifications

For each confirmed notification state trigger, recipient, channel, timing, content owner, delivery/failure semantics, preferences/opt-out and audit needs. Explain absence when Not applicable.

## Search

Describe search purpose, fields, filters/sorts, expected volume, matching needs, freshness and permission filtering. Keep engine selection in Architecture and distinguish required search from an optional later feature.

## Reporting

Define audience, decisions supported, KPI formula/source, periods, filters, freshness and historical reproducibility. Detail accepted export format/fields/access/size/delivery/retention requirements.

## Integrations

Describe actual systems, purpose/owner, direction, authoritative source, synchronization/acceptable lag, data shared, failure result and known dependencies. Link technical contracts and security controls; mark unverified feasibility explicitly.

## Data Lifecycle

Describe creation, state transitions, retention, history/archive, deletion/export and ownership changes. Distinguish company records from personal accounts, historical reconstruction from logs, and legal/privacy-validation items from accepted business policy.

## Edge Cases

Document meaningful invalid input, permission denial, empty/missing records, duplicate/concurrent operations, late events, provider downtime, cancelled/deleted records and irreversible transitions only where they affect actual workflows. Link FR/BR/SEC and expected outcomes.

## Non-functional Requirements

Record performance/volume/concurrency, availability/maintenance, reliability/recovery, growth, accessibility, devices/connectivity and localisation expectations. Give measurement context, source and status; preserve estimates as estimates.

## MVP Scope

List accepted Must requirements and necessary controls/enablers, first usable outcome, any deliberately manual steps, launch acceptance and dependencies. Identify blocked requirements and explicitly bounded unknowns.

## Future Scope

Separate Should/Could items, long-term vision and unaccepted proposals. Do not make future scope a commitment without acceptance.

## Assumptions

List explicit Assumption records with ID when useful, source/reason, impact, owner, validation checkpoint and affected requirements. Do not promote them to facts elsewhere.

## Open Questions

List remaining questions with owner, impact, checkpoint, dependent requirements and acknowledged deferral/blocking status. Link Decisions for unaccepted choices.

## Glossary

Define domain terms and ambiguous labels using accepted language. Keep roles, entities, workflow states and integration names consistent across all documents.
