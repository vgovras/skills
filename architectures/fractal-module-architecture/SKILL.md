---
name: fractal-module-architecture
description: Use when creating or reorganizing application architecture into recursive feature modules with top-level command/query actions, or interchangeable providers hidden behind a module's public API.
---

# Fractal Module Architecture

- Keep `{actions,modules,common}/` at the application source root. Only `modules/` and `common/` may recur within modules; nest submodules by responsibility.
- `modules/<feature>/`: reusable or potentially reusable feature logic; keep related files together, not in root-level role folders.
- `common/`: shared modules; extract shared logic here instead of duplicating it.
- `actions/<name>.{command,query}.ts`: use-case-specific logic and module orchestration; commands change state, queries read data. Actions exist only in this top-level directory, never inside feature modules or `common/`.
- No direct imports between separate feature modules at any depth; coordinate them through top-level actions, including workflows involving nested modules. Nesting does not introduce another action scope.
- A nested `modules/<feature>/` is a separate feature boundary: parent, child, and sibling features must not import or re-export one another, including type-only imports. Top-level actions may use a nested feature's own public API directly. Ordinary files and providers within one feature are internal parts, not separate features.
- Dependencies: actions → modules/common; modules → common; common must not depend on feature modules or actions. Modules must not import actions or move use-case orchestration into services or factories to bypass this boundary.
- Expose each feature through `<feature>.module.ts` (factory and public API) and `<feature>.types.ts` (public contracts). Actions must not reach into another feature's service, repository, or providers. Preserve an established equivalent public entry point when one already exists.
- Keep feature-specific types in their owning feature. Put genuinely shared contracts in the nearest `common/` visible to all consumers; define them there rather than importing or re-exporting feature types from `common/`. Actions translate between distinct feature contracts.
- Application bootstrap may import public feature factories and actions to construct and inject dependencies and register handlers. Bootstrap performs no business workflow; a feature factory wires only its own internals and common dependencies. Injecting another feature or a workflow callback into a feature is not a workaround for the dependency boundary.
- Use `kebab-case` for files and folders; module file template: `modules/<feature>/<feature>.{service,controller,repository,types,constants}.ext`.

## Interchangeable Providers

- Use Strategy when one capability needs interchangeable implementations behind the same contract; avoid adding providers for a single implementation without a concrete need for alternatives.
- Structure: `<feature>/providers/<provider>/<provider>.provider.ts` and `<feature>/<feature>.{types,service,config,module}.ts`.
- Define `I<Feature>Provider` in `.types.ts`; each provider implements the same operations and observable semantics, hiding implementation details.
- Keep shared logic in `.service.ts`; depend on the injected provider contract, not concrete implementations or provider-specific branches.
- Read and validate provider selection and settings in `.config.ts`; select, create, and inject the provider in `.module.ts` using a factory. Reject unsupported selections.
- Keep provider composition inside the owning module; consumers use its public API without knowing the selected implementation.
- Normalize provider-specific results, errors, and events inside the owning module. Expose opaque references or deduplication keys when needed; consumers must not select providers, branch on provider names, or interpret vendor payloads. Provider settings belong to configuration and composition, not business API arguments.
- Providers are internal implementations of one module, not independent feature modules; internal wiring may import them without enabling cross-feature imports.

```text
<feature>/
├── providers/
│   ├── <provider-a>/
│   │   └── <provider-a>.provider.ts
│   └── <provider-b>/
│       └── <provider-b>.provider.ts
├── <feature>.types.ts
├── <feature>.service.ts
├── <feature>.config.ts
└── <feature>.module.ts
```
