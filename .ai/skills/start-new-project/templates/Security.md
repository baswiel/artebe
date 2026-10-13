# Security

Authoring contract: generate docs/Security.md from actual trust boundaries, data and accepted controls after review. Replace instructions with project content and justify Not applicable. Mark privacy/legal validation needs; do not make binding legal claims or copy this contract into output.

## Security Context

Describe actors, protected assets, company/user boundaries, exposed entry points and consequence of misuse. Distinguish planned controls from existing/verified implementation.

## Authentication

Record accepted registration/invitation/provisioning, verification/reset/recovery, required factors/SSO and service/API identity journeys. State baseline capability versus project requirement and lifecycle owners.

## Authorization

Provide actor/action/record scope/conditions/denied cases aligned with Product. Cover delegation, membership loss, cross-company/support/impersonation and export/search access if applicable; assign enforcement/control owners.

## Sensitive Data

Classify actual secret, financial, confidential and other sensitive categories, purposes, sources, boundaries and permitted readers. Use sanitized categories, never credentials or real records.

## Personal Data

Provide Data type, Purpose, Source, Who can access, Retention, Deletion, Export, Legal/business requirement and Validation owner/status. Separate asserted business/legal requirements from qualified validation still needed.

## Data Retention

State periods or lifecycle triggers, policy source/owner, audit/history/backup impact and exceptions/legal holds. Unknown periods remain owned questions; do not manufacture a universal policy.

## Data Export

Define entitled requester, identity/authorization, fields, delivery, format, access lifetime and traceability for required privacy/customer exports.

## Data Deletion

Distinguish account removal, company-record deletion, anonymisation, archival and final erasure. Reconcile shared/history/backup/provider copies and validation dependencies.

## Secrets

Identify credential categories, responsible owners, provisioning/environments, privileges, rotation/revocation and redaction. Store no values or access tokens in this document.

## File Uploads

State allowed content/size, ownership/access, validation/scan/sanitization requirements, private/public storage boundaries, processing rejection, delivery, retention and deletion for actual files.

## API Security

Describe actual clients, auth/abilities, expiry/revocation, ownership filtering, version/error contract, webhook authenticity/replay and credential responsibility. Do not treat public embed identifiers as privileged secrets.

## Rate Limiting

Identify abuse-sensitive inbound flows and relevant provider quotas, chosen limits or decision owner, key/scoping rationale and enforcement verification. Keep unconfirmed numbers Proposed.

## Logging

Define safe diagnostic fields/redaction, permitted readers, error tracking, retention and incident troubleshooting. Exclude tokens, complete payloads and unnecessary personal data.

## Audit Trail

Specify meaningful recorded actions/changes, original/delegated actor, old/new state or event, reasons, time, readers, integrity and retention. Keep business proof separate from diagnostic logs.

## External Integrations

Record data sharing/source of truth, credential owner, environment isolation, authenticity, failure/retry/duplicate handling and relevant provider/privacy validation. Link Architecture contracts.

## Threats

Derive threats from actual workflows/boundaries. For each record entry point, protected asset, misuse/failure scenario, consequence, required control and responsible owner; do not list generic threats as known project incidents.

## Required Controls

Use SEC identifiers with threat/requirement, specific control, scope, owner, status, architecture treatment, roadmap phase/task and test/review acceptance. Coverage must include relevant authorization, input, data, secrets/files/API, diagnostic and recovery controls.

## Open Security Questions

List unresolved access, identity recovery, sensitive-data, retention/legal, provider/file and operational questions with owner, impact, checkpoint and blocked work. Preserve the user's acknowledgment of bounded unknowns.
