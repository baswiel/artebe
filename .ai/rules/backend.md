---
globs: ["app/**/*.php", "routes/**/*.php", "bootstrap/**/*.php", "config/**/*.php"]
---

# Backend

Do:

- Keep controllers focused on input, authorization, the operation and its response; use architecture.md for extraction decisions.
- Prefer operation-specific Form Requests for new write forms. Keep small endpoint-specific inline validation when extraction adds no clarity.
- Put HTTP input rules at the request boundary; keep shared domain invariants with the operation that enforces them.
- Pass only the validated fields intended for the operation. Exclude control fields and assign protected values explicitly.
- Apply security.md's authorization before reading protected data or starting side effects.
- Distinguish forbidden access, invalid input and integration failure in the response; preserve the appropriate status.
- Keep route names grouped by the actual resource/feature and standard controller action. Split route files only at meaningful boundaries.
- Follow Boost's API Resources and versioning rules. Keep serialization separate from queries and business decisions; expose only the required contract fields.
- Follow database.md for enum/cast choices; preserve public enum values when evolving an HTTP contract.
- Register new middleware, routing and exception policy in the current `bootstrap/app.php` structure.
- Report unexpected failures through the existing exception/Sentry integration once, with security.md's safe context.

For integrations and background work:

- Put external protocols behind focused clients/services; configure timeouts and distinguish transport, provider and business failures.
- Retry only recoverable failures; account for duplicate side effects before retrying writes.
- Configure an actual asynchronous connection and running worker before relying on background execution.
- Set retry, timeout and terminal-failure behavior for jobs that perform external work. Align execution time with the selected driver's reservation/visibility behavior.
- Make retried side effects idempotent and re-check relevant resource state when a queued job executes.
- Follow database.md for dispatch and external side effects relative to commit.
- Define schedules in the current console routing/bootstrap structure; match scheduler invocation frequency to the required cadence.
- Prevent harmful schedule overlap. Use a shared lock when multiple nodes must produce only one execution.
- Define cache keys, expiration and invalidation for cached domain data; include tenant/permission context where results depend on it.
- Read application settings through config; restrict environment-variable reads to configuration files.

Do not:

- Add a generic empty BaseRequest or reproduce Blackbirds' Validator/Persistor protocol for new forms.
- Convert authorization or unexpected failures into empty validation errors or successful responses.
- Rebuild Fractal/custom Transformer infrastructure when Laravel's existing response tools suffice.
- Assume Redis, RabbitMQ, Horizon or a scheduler is active because a package or connection exists.
