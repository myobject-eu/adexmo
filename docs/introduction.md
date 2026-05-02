# Introduction

## What is ADExMo?

ADExMo stands for **Action-Driven Execution Model**.

It is an architectural execution model where the business behavior of an application is defined as a set of executable actions, independent from any specific interface.

In ADExMo, the core question is not:

```text
Which controller, command, or job should contain this logic?
```

The core question is:

```text
What action does the system need to execute?
```

An action represents a real use case of the application.

Example:

```pseudo
createUser(email: string, admin: bool = false)
```

This action can later be invoked by:

- an HTTP endpoint
- a CLI command
- a background job
- a test runner
- an integration adapter

The interface changes.

The action remains the same.

---

## The Problem ADExMo Solves

In many applications, business logic is spread across different entry points.

A typical backend application may contain:

- HTTP controllers
- CLI commands
- queue jobs
- scheduled tasks
- API endpoints
- service classes

Over time, each interface starts adding its own logic, validation, mapping, and special cases.

This creates common problems:

- duplicated business logic
- inconsistent behavior between interfaces
- difficult testing
- unclear responsibility boundaries
- late validation of core behavior
- strong dependency between backend, API, and UI teams

The result is that the real application behavior is not always visible in one place.

Even worse, the business logic is often not directly executable until an interface exists.

This is a structural weakness.

ADExMo addresses this weakness by making the business logic executable first.

---

## Core Principle

ADExMo is based on a simple principle:

> The Service Layer must be the single execution point of the application.

This means that controllers, commands, jobs, and other interfaces must not own business logic.

They should only invoke executable actions.

In ADExMo:

```text
Interface -> Action
```

Not:

```text
Interface -> Logic
```

The action is the stable execution unit.

The interface is only an adapter.

---

## What is an Action?

An action is a directly executable unit of business logic.

It represents something the system can do.

Examples:

```text
createUser
processOrder
generateInvoice
assignPermission
createEnvironment
```

A valid ADExMo action must:

- represent a real business use case
- have a clear intent
- belong to a domain
- define explicit inputs
- produce a predictable output
- avoid implementation details
- be executable without HTTP, CLI, UI, or queue context

An action is not a technical step.

For example:

```text
fetchDataAndSaveToDatabase
```

is not a good ADExMo action because it exposes implementation details.

A better action would be:

```text
generateReport
```

The action defines what the system does, not how it does it.

---

## Traditional Model vs ADExMo

### Traditional Model

In a traditional application, multiple interfaces may call the same service layer in different ways.

```text
HTTP Controller  -> Service
CLI Command      -> Service
Queue Job        -> Service
```

This may look clean at first, but each entry point can introduce different behavior.

For example:

- the controller validates one set of fields
- the CLI command uses different defaults
- the queue job skips some checks
- the test calls the service directly with another structure

The system slowly becomes inconsistent.

---

### ADExMo Model

In ADExMo, all interfaces invoke the same action.

```text
HTTP Controller  -> Action
CLI Command      -> Action
Queue Job        -> Action
Test Runner      -> Action
```

The behavior is centralized.

The execution path is clear.

The action becomes the contract.

---

## Interface Adapters

ADExMo does not remove interfaces.

It gives them a cleaner role.

An interface adapter is responsible for:

1. receiving input
2. mapping input to action parameters
3. invoking the action
4. returning the result

An adapter must not contain business behavior.

Examples of adapters:

- HTTP adapter
- CLI adapter
- Queue adapter
- Test adapter
- Integration adapter

This makes the interface replaceable.

The system can expose the same behavior through different channels without duplicating logic.

---

## The Actions List

The ADExMo Actions List is the official catalog of application behavior.

It defines what the system can do.

A typical Actions List contains:

- domains
- action names
- intent
- input parameters
- output description
- business rules
- constraints
- executable signatures

Example:

```markdown
## Domain: user

### Action: user:create

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

The Actions List is not just documentation.

It is the contract of application behavior.

It is used by:

- analysts
- backend developers
- API developers
- UI developers
- testers
- integration teams

Everyone works from the same behavioral contract.

---

## Domains

Domains organize actions by responsibility.

A domain is a logical area of the system.

Examples:

```text
user
order
invoice
permission
environment
```

Domains must not describe technical layers.

Invalid domains:

```text
controller
api
database
repository
```

A domain answers the question:

```text
Which area of responsibility does this action belong to?
```

Not:

```text
Where is this implemented?
```

This keeps the Actions List readable and scalable.

---

## Why ADExMo Matters

ADExMo is useful because it changes the development sequence.

Instead of starting from interfaces, the team starts from executable behavior.

Traditional sequence:

```text
UI or API -> Controller -> Service -> Test
```

ADExMo sequence:

```text
Action -> Test -> Adapter -> UI or API
```

This has practical benefits:

- business logic can be tested earlier
- backend work does not need to wait for UI
- frontend teams can integrate on stable behavior
- API teams can map endpoints to actions
- CLI tools can be generated or connected more easily
- behavior remains consistent across interfaces

The main benefit is not theoretical elegance.

The main benefit is operational clarity.

---

## What ADExMo Is Not

ADExMo is not a framework.

It does not replace Laravel, Django, Symfony, FastAPI, Spring, or any other framework.

ADExMo is not a programming paradigm.

It does not replace Object-Oriented Programming or Functional Programming.

ADExMo is not an API specification format.

It does not replace OpenAPI.

ADExMo defines the execution model of business logic.

Frameworks, APIs, UIs, and implementation paradigms remain free choices.

---

## Relation to FastCLI

FastCLI is related to ADExMo, but it is not the same thing.

FastCLI started from the idea of defining CLI commands through function signatures and metadata.

ADExMo generalizes that idea.

The CLI is only one possible adapter.

The broader principle is:

> Actions are the execution unit. Interfaces are adapters.

FastCLI can be used as a CLI adapter for ADExMo actions.

But ADExMo is not a CLI tool.

It is an architectural execution model.

---

## When to Use ADExMo

ADExMo is especially useful when an application has:

- complex business logic
- multiple interfaces
- API and UI teams working in parallel
- CLI or automation needs
- background jobs
- integration requirements
- testability problems
- repeated logic across controllers, commands, and jobs

It is particularly effective for:

- ERP integrations
- iPaaS platforms
- automation systems
- backend-heavy applications
- admin portals
- multi-team projects
- systems where business behavior must be clear and versioned

---

## When ADExMo May Be Too Much

ADExMo may be unnecessary for very small applications where:

- there is only one interface
- business logic is minimal
- the application is mostly CRUD
- there is no need for separate execution paths
- the team is very small and the domain is simple

In these cases, using ADExMo formally may introduce more structure than needed.

The model is most valuable when behavior, responsibility, and integration must remain clear over time.

---

## Summary

ADExMo defines application behavior as executable actions.

It separates:

```text
what the system does
```

from:

```text
how the system exposes it
```

The result is a model where business logic is:

- explicit
- executable
- testable
- reusable
- interface-independent
- easier to integrate

Core idea:

> One execution layer. Any interface.
