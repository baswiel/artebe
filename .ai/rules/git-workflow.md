---
globs: [".github/**/*", ".circleci/**/*", ".git*", "commitlint.config.*", "commitzen.js", "package.json"]
---

# Git workflow

Do:

- Use short-lived branches from the project's agreed base branch; include a ticket reference when one exists.
- Preserve deployment branches such as staging/production only where release configuration uses them.
- Use Conventional Commits: `<type>(<scope>): <imperative summary>`, with a meaningful scope, lowercase subject and at most 72 characters.
- Mark breaking changes with `!` and a `BREAKING CHANGE:` body/footer explaining impact; add `Refs:` when a ticket applies.
- Keep each commit/PR focused on a coherent change; exclude secrets and generated ignored artifacts.
- Describe the problem, resulting behavior, verification and relevant migration/security/operational risks in a PR.
- Require quality.md's applicable gate before merge/release; inspect actual repository checks and settings before claiming enforcement.
- At task completion, suggest 2–3 Conventional Commit messages and mark one Recommended, as required by the Rapide instructions.

Do not:

- Infer a mandatory branch prefix, ticket system, reviewer count or merge strategy from a few historic branches/commits.
- Introduce an elaborate Git-flow or claim branch protection exists without checking repository settings.
- Bypass a failing required check without an explicit project decision.
