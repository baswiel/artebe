---
globs: ["**/*", ".ai/**/*", ".github/**/*", ".circleci/**/*", ".docker/**/*", ".kubernetes/**/*", ".env*"]
---

# Rapide rules index

Do:

- Read AGENTS.md and this index before planning or editing.
- Read every rule file mapped to the paths and concerns in scope. Globs are a lookup map, not automatic enforcement.
- Search `.ai/rules` for task keywords to find cross-cutting requirements.
- Inspect siblings, existing components, scripts and project documentation before adding structures.
- Apply explicit project-specific agreements in `docs/` before generic Rapide conventions, only within their stated scope. Keep user instructions and Boost requirements in force.
- Treat [the audit](../../docs/rapide-architecture-audit.md) as evidence and decision rationale, not additional framework instructions.
- Add security.md for authentication, permissions, personal data, uploads, input handling or external integrations, regardless of path.
- Add quality.md for verification, toolchain, deployment or dependency changes, regardless of path.
- Read existing `.ai/rules/boost` files if present; this template does not create or require that directory.

| Rule | Apply when | Paths / globs |
| --- | --- | --- |
| [architecture.md](architecture.md) | Choose responsibility boundaries or abstractions | `app/**/*.php`, `routes/**/*.php`, `bootstrap/**/*.php`, `config/**/*.php`, `tests/**/*.php` |
| [backend.md](backend.md) | Change HTTP flows, integrations, jobs, schedules or cache behavior | `app/**/*.php`, `routes/**/*.php`, `bootstrap/**/*.php`, `config/**/*.php` |
| [database.md](database.md) | Change schema, persistence, queries or database fixtures | `database/**/*.php`, `app/**/*.php`, `tests/**/*.php`, `config/database.php` |
| [frontend.md](frontend.md) | Change pages, UI, forms, types or frontend state | `resources/js/**/*`, `resources/css/**/*`, `resources/views/**/*`, `vite.config.*`, `app/Http/**/*.php` when changing Inertia props |
| [testing.md](testing.md) | Select, write or review regression coverage | `tests/**/*.php`, `database/factories/**/*.php`, `phpunit.xml`, `.env.testing`, `.env.ci`; add for behavior changes in any path |
| [security.md](security.md) | Change a trust boundary or handle sensitive data | `app/**/*.php`, `routes/**/*.php`, `resources/**/*`, `config/**/*.php`, `bootstrap/**/*.php`, `database/**/*`, `tests/**/*`, `.env*`, `.docker/**/*`, `.kubernetes/**/*`, `infra/**/*` |
| [quality.md](quality.md) | Verify a change or change tooling/operations | `composer.*`, `package*.json`, `pnpm-*.yaml`, `pint.json`, `eslint.config.*`, `.prettier*`, `tsconfig*.json`, `vite.config.*`, `phpunit.xml`, `.github/**/*`, `.circleci/**/*`, `.docker/**/*`, `.kubernetes/**/*`, `infra/**/*`, `Dockerfile*`, `docker-compose*`, `Makefile`, `.make/**/*` |
| [git-workflow.md](git-workflow.md) | Prepare branches, commits, PRs, merges or releases | `.github/**/*`, `.circleci/**/*`, `.git*`, `commitlint.config.*`, `commitzen.js`, `package.json`; all repository changes when preparing a commit or PR |

For `docs/**/*.md` and `.ai/rules/*.md`, read this index and the domain rules for the conventions being documented; reconcile changes with the audit and AGENTS.md.

Do not:

- Duplicate Boost syntax, generators, package-version instructions or formatter rules here.
- Treat patterns observed in the references as requirements without the audit's chosen scope.
- Create speculative folders, packages or boilerplate classes to satisfy a rule.
