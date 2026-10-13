---
globs: ["database/**/*.php", "app/**/*.php", "tests/**/*.php", "config/database.php"]
---

# Database

Do:

- Add a new migration for changes to deployed/shared schema; keep fresh installation and upgrade behavior aligned.
- Define foreign keys and delete behavior from ownership and retention requirements. Do not cascade by habit.
- Use nullable columns only for meaningful absent/unknown states; align request validation and casts with that meaning.
- Enforce persistent uniqueness and relationship invariants in the database where applicable.
- Add indexes for concrete filtering, joins or ordering; check composite/redundant indexes and the actual query plan when performance matters.
- Keep normal timestamps unless the table has an explicitly different lifecycle.
- Use soft deletes only for an actual restoration/retention requirement; define final deletion of sensitive data separately.
- Use the template's `casts()` convention for boolean, date, structured and enum values.
- Model relationships with Eloquent; use local scopes for repeated filters before extracting a larger query abstraction.
- Eager-load the relationships required by serialization or iteration. Use counts/aggregates when rows are unnecessary.
- Bound list queries with pagination or chunking; choose stable ordering where pagination requires it.
- Keep raw SQL or complex joins only where the query requires them; bind values and allowlist user-selected sort/filter identifiers.
- Use factories and meaningful states for reusable test data. Keep seeders for explicit reference/demo/setup data, with environment-specific execution.
- Make repeatable setup seeders safe to rerun; keep production reference data separate from demo users and credentials.

Transactions:

- Let the business operation own a transaction when multiple writes must succeed together.
- Prefer Laravel's transaction callback to manual begin/commit/rollback for normal operations.
- Lock or atomically update mutable state when concurrent execution could violate an invariant.
- Dispatch jobs that need committed rows after commit. Defer external side effects until commit or provide explicit compensation/recovery.
- Treat mail, provider calls and filesystem writes as separate from database rollback.

Do not:

- Create a transaction for every model save or hold locks during slow external calls.
- Reimplement soft-delete behavior with runtime schema inspection or universal custom Builders.
- Add UUIDs, SoftDeletes, DTOs or model interfaces to every model.
- Use application models to define historical schema migrations.
- Require a factory or seeder for every runtime data change.
