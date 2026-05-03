# ADExMo

**Action-Driven Execution Model**

ADExMo is an architectural execution model where application behavior is defined as executable actions, independent from any specific interface such as HTTP, CLI, UI, or background jobs.

Its core principle is simple:

> One execution layer. Any interface.

---

## What is ADExMo?

ADExMo defines the business logic of an application as a structured set of executable actions.

An action represents a real use case of the system.

Example:

```pseudo
createUser(email: string, admin: bool = false)
```

The same action can be invoked by different interfaces:

- an HTTP controller
- a CLI command
- a background job
- a test runner
- an integration adapter

The interface does not contain business logic.

The interface only invokes the action.

---

## The Problem

In many applications, business logic is spread across multiple entry points:

- controllers handle HTTP requests
- commands handle CLI execution
- jobs handle asynchronous processing
- services are called differently by each interface

This often leads to:

- duplicated logic
- inconsistent behavior
- difficult testing
- unclear responsibility boundaries
- late validation of core behavior
- dependency between backend, API, and UI teams

In practice, the application logic is often not truly executable until an interface exists.

ADExMo addresses this by making the business logic directly executable from the beginning.

---

## The ADExMo Principle

ADExMo is based on the following idea:

> The Service Layer must be the single execution point of the application.

In ADExMo:

- actions contain business behavior
- adapters receive input and call actions
- the runtime manages execution
- interfaces do not own business logic

---

## Core Concepts

### Action

An action is a directly executable unit of business logic.

It must:

- represent a real use case
- have a clear business intent
- define explicit inputs
- produce a predictable output
- be executable without depending on HTTP, CLI, or UI context

Example:

```pseudo
processOrder(orderId: int, force: bool = false)
```

---

### Domain

A domain is a logical group of actions that represents an area of responsibility in the system.

Examples:

```text
user
order
invoice
environment
permission
```

Domains are not technical layers.

Invalid domains:

```text
controller
database
api
repository
```

A valid domain describes responsibility, not implementation.

---

### Actions List

The Actions List is the official contract of application behavior.

It defines:

- available domains
- available actions
- input contracts
- output contracts
- business rules
- constraints
- executable signatures

The Actions List is versioned and acts as the shared contract between analysis, development, API, UI, and integration teams.

---

### Interface Adapter

An adapter connects an external interface to an action.

Examples:

- HTTP adapter
- CLI adapter
- Queue adapter
- Test adapter
- Integration adapter

Adapters do not contain business logic.

They only:

1. receive input
2. transform it into action parameters
3. invoke the action
4. return the result

---

### Execution Runtime

The execution runtime is responsible for the execution lifecycle.

Typical responsibilities:

- action resolution
- parameter binding
- validation
- context handling
- execution
- output handling

Generic execution flow:

```text
Input
Resolution
Binding
Validation
Execution
Output
```

---

## Before and After ADExMo

### Traditional Model

```text
HTTP Controller  -> Service
CLI Command      -> Service
Queue Job        -> Service
```

Each interface may introduce its own mapping, validation, and behavioral differences.

### ADExMo Model

```text
HTTP Controller  -> Action
CLI Command      -> Action
Queue Job        -> Action
Test Runner      -> Action
```

There is one execution path.

---

## Example

### Define an action

```pseudo
action user:create

Intent:
Create a new user account.

Input:
- email: string
- admin: bool = false

Output:
- created user
- validation error

Signature:
createUser(email: string, admin: bool = false)
```

### Invoke from HTTP

```pseudo
POST /users

runAction("user:create", {
    email: request.email,
    admin: request.admin
})
```

### Invoke from CLI

```pseudo
app user:create test@example.com --admin

runAction("user:create", {
    email: "test@example.com",
    admin: true
})
```

Both interfaces invoke the same action.

The behavior remains consistent.

---

## What ADExMo Is

ADExMo is:

- an execution model
- a contract-based approach to business logic
- a way to make the Service Layer directly executable
- a method to separate business behavior from interfaces
- a structure for analysis, implementation, and integration

---

## What ADExMo Is Not

ADExMo is not:

- a framework
- a programming language
- a replacement for Laravel, Django, Symfony, FastAPI, or other frameworks
- an alternative to OOP or Functional Programming
- a UI architecture
- an API specification format

ADExMo can be implemented using different languages, frameworks, and programming paradigms.

---

## Relation to FastCLI

FastCLI is a CLI adapter concept related to ADExMo.

Initially, FastCLI explored the idea of defining CLI commands through function signatures and metadata.

ADExMo generalizes that idea.

The CLI is only one possible interface.

The real model is broader:

> actions are the execution unit, and interfaces are adapters.

---

## Repository Structure

```text
adexmo-spec/
├── README.md
├── LICENSE
├── CHANGELOG.md
├── CONTRIBUTING.md
│
├── docs/
│   ├── introduction.md
│   ├── core-concepts.md
│   ├── execution-model.md
│   ├── actions-list.md
│   ├── domains.md
│   ├── action-definition-checklist.md
│   ├── contract-based-development.md
│   ├── from-system-operations-to-actions.md
│   ├── application-execution-analyst.md
│   ├── adexmo-vs-oop-vs-functional.md
│   └── glossary.md
│
├── examples/
│   ├── generic/
│   │   └── sample-actions-list.md
│   └── laravel/
│       └── minimal-setup.md
│
├── templates/
│   ├── action-definition-template.md
│   ├── actions-list-template.md
│   ├── domain-definition-template.md
│
└── assets/
    └── diagrams/
```

---

## Documentation

Main documentation:

- [Introduction](docs/introduction.md)
- [Core Concepts](docs/core-concepts.md)
- [Execution Model](docs/execution-model.md)
- [Actions List](docs/actions-list.md)
- [Domains](docs/domains.md)
- [Action Definition Checklist](docs/action-definition-checklist.md)
- [Contract-Based Development](docs/contract-based-development.md)
- [ADExMo vs OOP vs Functional Programming](docs/adexmo-vs-oop-vs-functional.md)
- [Glossary](docs/glossary.md)

---

## Examples

Practical examples are available in the `examples/` directory.

Current examples:

- [Generic Actions List](examples/generic/sample-actions-list.md)
- [Applying ADExMo in Laravel](examples/laravel/minimal-setup.md)

---

## Basic Rules

An ADExMo action must:

1. represent a real business use case
2. belong to exactly one domain
3. have a clear verb and object name
4. define explicit input parameters
5. produce a predictable result
6. avoid implementation details
7. be executable without any interface
8. remain independent from HTTP, CLI, UI, or queue context

If an action exposes implementation details, it is not a valid ADExMo action.

---

## Versioning

The ADExMo specification should be versioned.

Recommended versioning model:

```text
MAJOR.MINOR.PATCH
```

Version types:

- `PATCH`: documentation fixes, clarifications, non-breaking changes
- `MINOR`: new concepts, new examples, new templates, non-breaking additions
- `MAJOR`: breaking changes in the model, terminology, structure, or contracts

---

## Status

ADExMo is currently in specification phase.

The current goal is to define:

- the conceptual model
- the documentation structure
- the Actions List format
- the domain model
- implementation examples
- practical templates

---

## Contributing

Contributions are welcome if they preserve the core principles of ADExMo.

Before proposing changes, contributors should verify that the proposal:

- keeps business logic independent from interfaces
- preserves the action-based execution model
- avoids framework-specific assumptions
- does not introduce implementation leakage into action definitions
- improves clarity, consistency, or practical adoption

See [CONTRIBUTING.md](CONTRIBUTING.md) for details.

---

## License

This documentation is released under the license specified in the [LICENSE](LICENSE) file.

Recommended license for documentation:

```text
Creative Commons Attribution-ShareAlike 4.0 International
CC BY-SA 4.0
```

---

## Summary

ADExMo defines application behavior through executable actions.

It separates:

```text
what the system does
```

from:

```text
how the system exposes it
```

The result is a cleaner execution model where business logic becomes directly testable, reusable, and independent from any interface.

> One execution layer. Any interface.
