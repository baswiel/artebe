---
globs: ["resources/js/**/*", "resources/css/**/*", "resources/views/**/*", "vite.config.*", "app/Http/**/*.php"]
---

# Frontend

Do:

- Follow the template's Vue/Inertia/TypeScript stack and lowercase directory names; use Boost for framework APIs.
- Keep screen orchestration in `pages`, application shells in `layouts`, shared UI in `components`, reusable stateful behavior in `composables` and shared contracts in `types`.
- Use `script setup` with TypeScript for new Vue components; type page props and shared payload contracts.
- Reuse existing UI primitives and domain components before creating alternatives.
- Keep a component local to its feature until genuine reuse justifies promotion.
- Choose the installed Inertia Form or useForm facility according to interaction needs; show field errors and prevent duplicate submissions while processing.
- Treat frontend validation as interaction assistance; retain authoritative validation and business decisions on the server.
- Keep state local, in form state or page props until cross-page lifecycle requirements justify a shared composable or store.
- Use the existing Wayfinder integration rather than transporting route methods/URLs in generic Form/URL DTOs.
- Regenerate route outputs through existing tooling after backend route changes; never edit generated route files.
- Apply security.md to shared props, HTML content and permission-dependent UI.

Do not:

- Install or use Livewire in Rapide projects; build interactive application UI with Vue and Inertia.
- Copy the reference repositories' Mix setup, old Inertia adapter configuration or uppercase folder convention into this template.
- Introduce Pinia/Vuex, global mixins or a duplicate client route registry without a concrete requirement.
- Put pricing, eligibility, authorization or persistence decisions exclusively in browser code.
