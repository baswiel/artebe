---
globs: ["app/**/*.php", "routes/**/*.php", "resources/**/*", "config/**/*.php", "bootstrap/**/*.php", "database/**/*", "tests/**/*", ".env*", ".docker/**/*", ".kubernetes/**/*", "infra/**/*"]
---

# Security

Do:

- Use the template's Fortify/session authentication; add an API-authentication mechanism only for an actual client requirement.
- Enforce authorization on the server before protected reads/writes and side effects. Use policies for resource/ownership rules, gates for non-resource abilities.
- Scope resource lookup and authorization to the relevant user/organisation/tenant; authentication or a valid identifier alone grants no ownership.
- Verify denial paths in the production-relevant context, including any environment or host bypass.
- Apply rate limits to sensitive auth/recovery flows, public submissions and expensive endpoints; verify the limiter is attached, not merely defined.
- Honor provider quotas where needed; keep outbound throttling separate from client authentication.
- Define token abilities, expiry and revocation for protected APIs. Keep privileged credentials on the server; treat browser/embed keys as public identifiers unless the contract establishes otherwise.
- Keep secrets in environment/secret stores consumed by config; use placeholders in examples and synthetic credentials/data in tests.
- Allowlist log/error-tracker context; redact credentials, tokens and personal data from request/response diagnostics.
- Minimize personal data in storage, logs and shared props; set retention/access according to the project's actual requirements.
- Restrict request-derived mass assignment to explicitly permitted fields; set ownership, roles and sensitive state on the server.
- Validate upload content/type and size, use storage-generated names, and require authorization for sensitive file access.
- Store untrusted uploads outside executable locations; use private storage for sensitive files. Apply scanning/sanitization when the file's risk and project policy require it.
- Keep CSRF protection on session-authenticated writes; limit exemptions to verified integration flows with their own authenticity checks.
- Escape output for its render context; allow raw HTML or `v-html` only behind an explicit trusted/sanitized content boundary.
- Disable debug exposure in production and keep secrets/sensitive fields out of model serialization and Inertia/API props.

Do not:

- Copy custom JWT/API-key/host bypasses from the references as a template default.
- Treat frontend visibility, CORS, client validation or host/origin headers alone as authorization.
- Pass `$request->all()` into persistence or unguard models to bypass input selection.
- Log API keys, tokens, complete lead/request payloads or recoverable sensitive data.
- Validate uploads solely by client filename extension or expose private files through a public disk.
- Disable CSRF globally or assume installed antivirus tooling proves uploads are scanned.
