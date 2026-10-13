---
globs: ["tests/**/*.php", "database/factories/**/*.php", "phpunit.xml", ".env.testing", ".env.ci"]
---

# Testing

Do:

- Apply Boost's test enforcement and testing-best-practices skill when changing behavior.
- Prefer Feature tests for HTTP, persistence and Laravel integration; use Unit for standalone deterministic logic.
- Name tests after the expected behavior and condition; follow the existing `test_...` convention.
- Cover the successful outcome and each relevant authorization, validation, business-rule and failure decision.
- Assert response, persisted state and meaningful side effects for writes; prove rejected operations do not produce forbidden changes.
- Test cross-user/tenant access when those boundaries exist; retain one endpoint assertion proving the policy is applied.
- Put exhaustive shared policy/rule cases at their owning layer; avoid repeating the full matrix through every endpoint.
- Use factories/states for database scenarios and synthetic fixtures for provider contracts.
- Run database tests in an isolated test database using project migrations and the established refresh strategy; use the production engine where driver-specific behavior matters.
- Fake external HTTP, mail, storage and queue boundaries as needed and assert the relevant result. Control time for expiry/retry behavior.
- Cover provider failure and retry/idempotency behavior where side effects depend on it.
- Keep live integration/contract tests explicitly opt-in and separate from the deterministic merge gate. Report skipped live checks as unverified.
- Add a targeted regression case for a fixed defect; run affected tests first and both application suites in the merge gate.

Do not:

- Target arbitrary coverage percentages or test framework declarations without application behavior.
- Mock Eloquent/query builders to replace testing actual persistence behavior.
- Depend on real provider credentials or network responses in normal CI tests.
- Change the app environment to local or disable authorization to make a security test pass.
- Hand-build substitute schemas merely to avoid maintaining valid project migrations.
