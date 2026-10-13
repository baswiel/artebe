# Access, privacy and security discovery

Use for phases 7, 9, 14–15 and related API/integration concerns. Read the project's security rules; capture project-specific decisions rather than reproducing a generic compliance checklist.

## Accounts and authentication

Inspect the existing authentication foundation first. Ask about account journeys and required assurance, not which library to install:

- Who receives an account: open registration, invitation, internal provisioning or an external identity system?
- Who grants/revokes access, and what happens when a staff member leaves or changes company?
- Are email ownership verification, password recovery and account reactivation needed in the confirmed journey?
- Which users/actions require a second factor, and how does recovery work? Existing 2FA support does not make it mandatory for every actor.
- Is SSO/passkey/OAuth required by an actual customer/provider? Which identity owner, recovery path and fallback apply?
- Are guests, service accounts or API clients involved? What can each do without a person signing in?
- If impersonation is needed, who may start it, for what reason, with what visibility, limits and recorded original actor?

Do not collect credentials or assume a supported feature is a required workflow. Route protocol feasibility to [integrations.md](integrations.md).

## Authorization

Build an action × actor × record-scope × condition matrix from core workflows. Include denied cases, membership changes, delegated access, platform support and bulk/export/search access when relevant.

Ask who can create, read, modify, assign, approve, cancel, export or erase each actual resource. Can a person act on another company's record? What happens after they switch organisation or lose membership? Does a URL/API identifier expose another user's record?

Distinguish authenticated identity, role, explicit permission, ownership and organisation/team/record scope. Prefer the simplest model supported by the rules; no automatic RBAC or universal super-admin. UI visibility and CORS are not server-side permission decisions.

## Data classification and privacy

Identify only categories, using sanitized examples: personal/contact data, special-category data, financial data, identifiers, addresses, location, IPs, cookies, logs, uploads and provider credentials as actually present.

For each data category capture:

| Field | Decision to obtain |
| --- | --- |
| Type and source | What is collected, from whom and at which boundary? |
| Purpose | Why is each field required; can less data serve the same need? |
| Access | Who can read/change/export it, including operators/providers? |
| Retention | Duration or triggering event, policy source and approving owner |
| Deletion | Erasure, anonymisation, archival, legal hold and backup treatment |
| Export | Subject/customer export need, verification and safe delivery |
| Requirement | Business/contract/legal basis claimed by the responsible owner |
| Validation | Which privacy/legal questions remain and who will validate them? |

Ask whether customer agreements or AVG/GDPR obligations affect location, subprocessors, access/export/deletion and retention. Record the user's stated requirement and the need for qualified review; do not provide a binding legal conclusion or invent retention periods.

Account deletion, removing a staff account and deleting company business records may be different operations. Reconcile them with historical proof and shared records using [data-model.md](data-model.md).

## Files and uploads — only if applicable

Determine allowed types/content, size and expected volume; public/private access; owner and download rights; storage/location constraints; thumbnails/conversion/extraction; malware risk and scan/sanitization needs; retention and final deletion.

Ask what happens while a file is processing or rejected, whether untrusted content is rendered inline, and whether a file can be shared outside its organisation. Investigate expiring access and audit only if the sharing requirement calls for them.

Explain processing/scan implications in user terms. Installed antivirus tooling is not proof of an active scan path; do not prescribe a scanner or storage provider without a need. Exports can be sensitive files even when no upload feature exists.

## API and integration protection

For actual clients clarify identity, credential owner, privileges, rotation/expiry/revocation, allowed data and abuse potential. Public embed keys may identify context but must not be treated as secret credentials for privileged operations.

For webhooks and imports ask how sender authenticity is verified, how duplicates/replays are handled and which data is trusted. Bound public/expensive endpoints and provider quotas using project-specific limits/unknowns, not invented numeric defaults.

Document secrets by reference/owner only. Confirm separate environments/test credentials and safe fixture/logging practices. Follow [integrations.md](integrations.md) for the rest of the contract.

## Logging, audit and threats

Ask what support needs to diagnose failures versus what business/legal history must prove. Identify permitted readers, safe correlation identifiers, redaction, retention and access to error tracking. Avoid full payload logging, tokens or unnecessary personal data.

Derive threats from the actual boundary, for example cross-company access, unauthorized state changes, duplicate effects, secret exposure, malicious upload/content or unsafe exports. These are prompts, not assumed project incidents.

For each relevant threat record entry point, protected asset, actor/capability, impact, required control, verification and owner. Assign SEC identifiers to controls and link them to affected FR/BR and roadmap tasks.

Distinguish planned controls, existing code evidence and verified configuration. "The system is secure" is not an acceptance criterion. An ownership denial test, rejected upload or secret-redaction test is observable.

## Open security questions

Do not silently choose global admin rights, retention, erasure/legal-hold conflicts, provider data sharing or identity recovery. Record an owner and decision checkpoint; block dependent implementation if an unresolved choice changes the safe design.

Before generation confirm the sensitive-data map, permission boundaries, major threats, required controls and privacy validation items with the user as part of final scope review.
