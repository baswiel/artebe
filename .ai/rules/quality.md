---
globs: ["composer.*", "package*.json", "pnpm-*.yaml", "pint.json", "eslint.config.*", ".prettier*", "tsconfig*.json", "vite.config.*", "phpunit.xml", ".github/**/*", ".circleci/**/*", ".docker/**/*", ".kubernetes/**/*", "infra/**/*", "Dockerfile*", "docker-compose*", "Makefile", ".make/**/*"]
---

# Quality gate

Do:

- Use the existing `composer ci:check` as the central local check command; inspect scripts before relying on their scope.
- Generate ignored Wayfinder artifacts through existing tooling before typechecking in a clean checkout; provide PHP/appbootstrap for generation.
- Require ESLint, Prettier, TypeScript, Pint and both PHPUnit suites in the merge/release gate through `composer ci:check`.
- Require `npm run build` and `composer audit` alongside the central command; assess and resolve dependency findings or record a scoped, time-bounded mitigation.
- Run affected checks first during development; run the complete applicable gate before merge.
- Keep CI checks non-mutating and aligned with the existing formatter/linter configs; use Boost's formatting workflow when editing PHP.
- Install from committed lockfiles (`composer install`, `npm ci`) with supported PHP/extensions and Node versions.
- Record missing dependencies, unavailable checks and skipped integration coverage explicitly; do not claim an unexecuted gate passed.
- Wire the defined gate into CI explicitly; a Unit-only job does not satisfy it. Rule files do not implement CI or branch protection.
- Verify required migrations, worker restart/rollout, scheduler cadence and application health when changing deployment behavior.
- Keep health responses free of sensitive diagnostics; do not infer provider availability from the template's `/up` endpoint.

Use PHPStan/Larastan when:

- A project has a concrete static-analysis need and an approved installation/configuration. Run its established gate thereafter.

Use Rector when:

- A scoped upgrade/refactor benefits from automated transformations and the resulting diff is reviewed and tested.

Do not:

- Invent a `composer check` command or assume PHPStan/Larastan, Rector or Qodana are active.
- Install dependencies or change scripts/CI solely because a rule mentions optional tooling.
- Skip Feature tests, ignore platform requirements in the normal gate or use a successful build as proof tests passed.
- Run automatic rewrites or formatter fixes as the CI validation step.
