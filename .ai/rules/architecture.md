---
globs: ["app/**/*.php", "routes/**/*.php", "bootstrap/**/*.php", "config/**/*.php", "tests/**/*.php"]
---

# Architecture

Do:

- Start with Laravel conventions and introduce abstractions only when the domain or infrastructure complexity justifies them.
- Keep simple endpoint-specific CRUD in a controller using Eloquent.
- Extract substantial calculations, shared behavior and external protocols from controllers.
- Group related code within existing Laravel directories when a real feature boundary emerges.
- Inject dependencies at the operation's entry point; introduce interfaces only for a useful boundary or interchangeable implementation.
- Use a transaction for writes that form one business invariant, not for every save; follow database.md for ownership and side effects.

Use an Action when:

- A named business use case contains meaningful orchestration, has multiple callers or becomes clearer to test independently.

Do not use an Action when:

- It only forwards a single CRUD call or creates one class per model by convention.

Use a Service when:

- Multiple operations share domain calculations or an external protocol needs a focused integration boundary.

Do not use a Service when:

- It is a generic pass-through layer around every model or duplicates an existing Action.

Use a repository or adapter when:

- A real storage/provider boundary needs isolation, a non-Eloquent protocol or interchangeable implementations.

Do not use a repository when:

- It is purely an Eloquent CRUD wrapper or exists only to mock database access.

Use a DTO/data object when:

- A complex payload needs normalization, immutable transfer or a useful typed boundary contract.

Do not use a DTO when:

- A small validated array or existing model already expresses the contract clearly.

Use a Query Object when:

- A substantial reusable query cannot be expressed clearly through relationships or local scopes.

Do not use a Query Object when:

- It adds a layer around a simple filter or duplicates a model scope.

Use Events/Listeners when:

- A domain occurrence has multiple independent reactions that benefit from decoupling.

Do not use Events/Listeners when:

- A direct local call expresses one required step more clearly.

Use a Job when:

- Work is slow, needs retries or must run outside the request. Apply backend.md's operational requirements.

Do not use a Job when:

- Quick synchronous logic or validation already meets the request's needs.

Use a Policy when:

- Permission depends on a resource, ownership or organisation/tenant context; follow security.md for enforcement.

Do not use a Policy when:

- It is an empty placeholder per model or a non-resource ability is clearer as a gate.

Use a custom Exception when:

- A meaningful domain or integration failure needs distinct handling by callers or the HTTP boundary.

Do not use a custom Exception when:

- Laravel's validation, authorization or missing-resource behavior already expresses the failure.

Do not:

- Pre-create module/domain trees, base repositories, model interfaces or generic Builder/Validator/Persistor frameworks.
- Add an abstraction package solely to establish a naming or invocation convention.
