---
name: ts-clean-code
description: Use when writing, modifying, reviewing, or refactoring JavaScript or TypeScript code (.{js,jsx,ts,tsx}) to apply rules for naming, conditions, loops, array iteration, and function or method design.
---

# TS Clean Code

## Index

- [Naming](#naming)
  - [General](#general)
  - [Variables and Parameters](#variables-and-parameters)
  - [Booleans](#booleans)
  - [Functions](#functions)
  - [Methods](#methods)
  - [Classes and Types](#classes-and-types)
  - [Enums](#enums)
  - [Constants](#constants)
  - [Files, Folders, and Modules](#files-folders-and-modules)
- [Conditions](#conditions)
- [Loops](#loops)
- [Functions and Methods](#functions-and-methods)
- [Types](#types)

## Naming

### General

- Follow project naming conventions unless a rule below specifies otherwise.
- Use precise, purposeful names; avoid unclear abbreviations and redundant context.
- Use one term per concept throughout the codebase.

### Variables and Parameters

- Name values by meaning and role; include units when the scale is ambiguous.
- Use plural names for collections.

### Booleans

- Use `!!x` instead of `Boolean(x)` for boolean conversion.
- Use `{is,has,can,should}` prefixes where language conventions allow.
- Prefer positive conditions; avoid double negatives.

### Functions

- Function name template: `<verb><Object?>` in `camelCase`, where the object is optional when the action is clear without it.
- Start action names with a verb; identify the object when needed.
- Describe behavior and significant side effects; avoid vague verbs without a specific purpose.

### Methods

- Name the action without repeating context clear from the owning type.
- Use consistent names for equivalent operations across types.

### Classes and Types

- Use nouns that describe the entity or responsibility; avoid vague names.
- Use `I<Name>` for interface names.
- Match class suffixes `Name{Service,Repository,Command,Query}` to file suffixes `.{service,repository,command,query}.ext`, respectively.
- For `.entity.ts` classes, use the entity name without an `Entity` suffix.

### Enums

- Use `CamelCase` for enum names and `UPPER_SNAKE_CASE` for keys.
- Match values to keys when possible; differ only when required.

### Constants

- Use `UPPER_SNAKE_CASE` for constant names.
- Name constants by meaning, not literal value; include units when relevant.

### Files, Folders, and Modules

- Name files, folders, and modules after their primary content or purpose.
- Use `kebab-case` for file and folder names.
- File name template: `<name>.{service,repository,scheme,types,constants,command,query}.ext`, where `<name>` describes the content in `kebab-case`; choose the suffix by the rules below.
- Use `.{service,repository}.ext` for application logic and data access, respectively.
- Use `<name>.entity.ts` for classes that encapsulate an entity's fields and synchronous logic that uses those fields; do not include asynchronous logic.
- Use `.scheme.ext` for validation and database schema files.
- Use `.{types,constants}.ext` for types/interfaces and constants/enums, respectively.
- When using CQRS, use `.{command,query}.ext` for commands that change application state and queries that read data, respectively.
- Replace `ext` with the actual file extension.

## Conditions

- Use `if` for boolean checks, ranges, compound conditions, or a small number of branches.
- Use `switch` for several explicit values of one expression when branches require distinct control flow; avoid unintended fallthrough.
- Use a lookup map `{ [key]: value }` or handler map `{ [key]: function }` when a key directly selects a value or action without additional branching.
- Define fallback behavior for unmatched cases or missing map keys; check that handlers exist before calling them.
- Use early `return` or `throw` for invalid inputs and exceptional cases; keep the main path flat instead of nesting it in `else` blocks.
- Use guard clauses and `continue` in loops to avoid nested conditional logic.

## Loops

- Extract processing logic into separate functions or methods; keep loop bodies focused on iteration, calls, and flow control.
- Use `for...of` to iterate values when you need side effects, `break`, `continue`, or sequential `await`.
- Use indexed `for` when the index, step size, iteration direction, or multiple positions matter.
- Use `while` when repetition depends on a condition rather than a known iteration count; use `do...while` when the body must run at least once.
- Use `for await...of` for asynchronous iterables.
- Use `map` to transform values, `filter` to select values, and `flatMap` to transform and flatten one level; use their returned arrays.
- Use `find` for the first matching value and `{some,every}` for boolean checks that stop once the result is determined.
- Use `reduce` to combine values into one result when it stays readable; otherwise use a loop.
- Use `forEach` only for synchronous side effects when early exit is unnecessary; never use it to await asynchronous work.
- Iterate object properties through `Object.{keys,values,entries}`; use `for...in` only when inherited enumerable properties are intentional.
- Use `Promise.all` with `map` for independent asynchronous operations when concurrency is safe; limit concurrency for large collections or constrained resources.
- Keep loop bodies flat with guard clauses and `continue`; use `break` when further iterations are unnecessary.

## Functions and Methods

- Give each function or method one responsibility.
- Extract independent pieces of logic into separate functions or methods.
- Prefer an argument object over positional parameters; destructure it in the signature as `{ key1, key2 }: TypeName` in TypeScript.
- Avoid mutating inputs unless required; make intended mutations explicit.
- Avoid boolean parameters that switch between different actions; use separate functions or explicit options instead.

## Types

- Set `compilerOptions.strictPropertyInitialization` to `false` in `tsconfig.json` instead of adding definite assignment assertions to class fields (`fieldName!: string`).
- Use `interface` for contracts implemented by classes with `implements`; use `type` otherwise.
- Do not use anonymous object type annotations `: { ... }`; declare and reference a named type instead.
- Avoid `any`; use `unknown` for untrusted values and narrow the type before use.
- Reuse existing types; do not duplicate definitions of the same concept.
- Use `import type` for imports used only as types.
- Derive types with utilities such as `Pick`, `Omit`, `Partial`, `Required`, and `Record` instead of repeating fields when their semantics fit.
