# Glossary

## Purpose

This glossary defines the main terms used in ADExMo.

Its purpose is to keep terminology consistent across documentation, ADRs, examples and implementation discussions.

ADExMo depends heavily on clear boundaries.  
Using the same terms with the same meaning is essential.

---

## ADExMo

**Action-Driven Execution Model**

ADExMo is an architectural execution model where application behavior is defined as executable actions, independent from any specific interface.

ADExMo defines what the system can execute.

It does not define how the internal implementation must be structured.

---

## Action

An action is a directly executable unit of business logic.

It represents a real use case of the system.

An action must:

- belong to one domain
- have a clear business intent
- define explicit inputs
- produce a predictable output
- be executable without depending on HTTP, CLI, UI or queue context

Example:

```pseudo
createUser(email: string, admin: bool = false)
```

---

## Action Definition

An Action Definition is the formal documentation of a single action.

It describes:

- name
- domain
- intent
- input
- output
- rules
- constraints
- signature

The Action Definition is part of the Actions List.

---

## Actions List

The Actions List is the official catalog of executable business actions.

It defines the behavior of the application in a structured and versioned form.

The Actions List acts as a contract between:

- analysis
- backend development
- API development
- UI development
- external integrations
- testing

The Actions List is the primary behavioral contract of ADExMo.

---

## Domain

A domain is a logical group of actions that represents an area of responsibility in the system.

A domain must describe responsibility, not implementation.

Valid examples:

```text
user
order
invoice
permission
environment
```

Invalid examples:

```text
controller
database
api
repository
```

Every action must belong to exactly one domain.

---

## Contract

A contract is a stable definition of what the system provides and how it can be invoked.

In ADExMo, the action contract includes:

- action name
- domain
- input parameters
- output expectations
- business rules
- constraints
- executable signature

The contract allows other teams to build APIs, UIs and integrations without knowing the internal implementation.

---

## Contract-Based Development

Contract-Based Development is the practice of using the Actions List as the reference contract for development.

With this approach:

- the core backend team defines and implements actions
- API teams expose actions through endpoints
- UI teams build interfaces based on action contracts
- external partners integrate without needing internal backend knowledge

This enables parallel work and clearer responsibility separation.

---

## Execution Interface

The Execution Interface is the point through which business logic is exposed and invoked.

In ADExMo, the execution interface is represented by actions.

This does not mean UI or HTTP API.

It means the operational boundary of the business logic.

---

## Execution Runtime

The Execution Runtime is the layer responsible for executing actions.

Typical responsibilities include:

- resolving the requested action
- binding input parameters
- validating input
- managing execution context
- invoking the action
- returning output

The runtime supports consistent execution across different adapters.

---

## Interface Adapter

An Interface Adapter connects an external interface to an ADExMo action.

Examples:

- HTTP adapter
- CLI adapter
- Queue adapter
- Test adapter
- Integration adapter

An adapter does not contain business logic.

It receives input, maps it to action parameters and invokes the action.

---

## Service Layer

The Service Layer is the application layer where business logic is executed.

In ADExMo, the Service Layer becomes explicit and directly executable through actions.

The Service Layer should be independent from interface-specific concerns.

---

## Business Logic

Business logic is the set of rules and behaviors that define what the application does.

In ADExMo, business logic belongs inside actions, not inside:

- controllers
- API routes
- UI components
- CLI commands
- queue handlers

Interfaces invoke business logic.  
They do not own it.

---

## Use Case

A use case is a meaningful operation the system performs for an actor or another system.

In ADExMo, a valid action must represent a real use case.

Examples:

```text
createUser
processOrder
generateInvoice
assignPermission
```

Invalid examples:

```text
handleData
executeProcess
manageEntity
callApiAndSave
```

---

## Signature

A signature is the executable representation of an action.

It defines the action name and input parameters.

Example:

```pseudo
processOrder(orderId: int, force: bool = false)
```

The signature must be clear, stable and implementation-independent.

---

## Input Contract

The Input Contract defines the parameters required to execute an action.

Inputs should be:

- explicit
- minimal
- typed
- business-oriented
- free from technical containers

Valid example:

```pseudo
processOrder(orderId: int, force: bool = false)
```

Invalid example:

```pseudo
processOrder(request: HttpRequest)
```

---

## Output Contract

The Output Contract defines the expected result of an action.

It should describe what the caller can expect after execution.

Examples:

- created user
- confirmed order
- generated invoice
- validation error
- authorization error

The output contract should be predictable and documented.

---

## Rule

A rule is a business condition applied by an action.

Example:

```text
An order cannot be processed if it has already been cancelled.
```

Rules belong to the action definition.

They should not be duplicated across UI, API and backend entry points.

---

## Constraint

A constraint is a condition or limitation that must be respected when executing an action.

Example:

```text
orderId must reference an existing order.
```

Constraints clarify the valid boundaries of execution.

---

## Validation

Validation is the process of checking whether input and execution conditions are acceptable before or during action execution.

In ADExMo, validation should be part of the execution model and should not be scattered across unrelated interfaces.

---

## Business Intent

Business Intent describes the purpose of an action in business terms.

It answers the question:

> What result is this action supposed to achieve?

A clear business intent helps prevent vague or technical action definitions.

---

## Implementation Detail

An implementation detail is an internal technical choice that should not appear in the action contract.

Examples:

- database query
- repository name
- controller method
- ORM model
- API client
- framework service
- internal algorithm

ADExMo actions define what the system does, not how it does it internally.

---

## Implementation Leakage

Implementation Leakage happens when technical details become visible in the action name, signature or contract.

Invalid examples:

```text
fetchDataAndSaveToDatabase
callExternalApiAndPersistResult
handleHttpRequest
```

Implementation leakage weakens the contract and couples the action to internal design.

---

## Interface Dependency

Interface Dependency occurs when business logic depends on a specific interface context.

Examples:

- an action depends on an HTTP request object
- a business rule exists only in a controller
- a CLI command applies different logic from the API
- a UI component decides backend behavior

ADExMo avoids interface dependency by making actions executable independently.

---

## CLI

CLI means Command Line Interface.

In ADExMo, CLI is only one possible adapter.

The CLI may be used to execute and test actions, but ADExMo is not a CLI model.

---

## FastCLI

FastCLI is a CLI adapter concept related to ADExMo.

It is based on defining CLI commands through function signatures and metadata.

FastCLI can be used to invoke ADExMo actions from the command line.

However, ADExMo is broader than FastCLI.

---

## HTTP Adapter

An HTTP Adapter maps HTTP requests to ADExMo actions.

It should:

- receive the HTTP request
- extract the required input
- invoke the correct action
- return the result

It should not contain business logic.

---

## Queue Adapter

A Queue Adapter maps asynchronous jobs or messages to ADExMo actions.

It allows the same action to be executed in the background without duplicating business logic.

---

## Test Adapter

A Test Adapter invokes actions directly for testing purposes.

This allows business logic to be tested without requiring:

- UI
- API
- HTTP routing
- queue infrastructure

---

## Integration Adapter

An Integration Adapter connects external systems to ADExMo actions.

It maps external input to action parameters and returns the result in the required format.

---

## Single Execution Path

Single Execution Path means that each business operation has one official execution point.

Instead of duplicating behavior across controllers, commands and jobs, every interface invokes the same action.

This improves consistency and testability.

---

## Single Source of Truth

In ADExMo, the action is the single source of truth for a business operation.

The same action may be invoked by:

- HTTP
- CLI
- queue
- tests
- external integrations

The behavior should remain consistent.

---

## Framework Independence

Framework Independence means that ADExMo does not depend on a specific framework.

It can be applied in:

- Laravel
- Django
- Symfony
- FastAPI
- Express
- other backend frameworks

Frameworks provide implementation infrastructure.  
ADExMo defines the execution model.

---

## Language Independence

Language Independence means that ADExMo is not tied to one programming language.

It can be implemented in:

- PHP
- Python
- JavaScript
- TypeScript
- Java
- C#
- other languages

The model remains the same even if syntax changes.

---

## OOP

OOP means Object-Oriented Programming.

It is an implementation paradigm based on objects, classes, encapsulation, inheritance and polymorphism.

ADExMo does not replace OOP.

OOP can be used to implement ADExMo actions.

---

## Functional Programming

Functional Programming is an implementation paradigm based on functions, immutability, composition and explicit input/output.

ADExMo does not replace Functional Programming.

Functional Programming can be used to implement ADExMo actions.

---

## Versioning

Versioning is the practice of tracking changes to the ADExMo specification or to an application's Actions List.

Recommended versioning model:

```text
MAJOR.MINOR.PATCH
```

Typical meaning:

- PATCH: clarification or non-breaking correction
- MINOR: non-breaking addition
- MAJOR: breaking change

---

## Breaking Change

A breaking change is a change that can break consumers of the action contract.

Examples:

- removing an action
- renaming an action
- changing required input parameters
- changing output behavior
- changing business rules in an incompatible way

Breaking changes should require a major version update.

---

## Non-Breaking Change

A non-breaking change is a change that does not break existing consumers.

Examples:

- adding an optional parameter
- improving documentation
- clarifying rules
- adding a new action
- adding examples

Non-breaking changes usually require a minor or patch version update.

---

## External Partner

An external partner is an outside team or company that builds part of the system using the Actions List as a contract.

Examples:

- UI development company
- API integration team
- mobile app development team
- third-party system integrator

ADExMo makes external collaboration easier because partners do not need internal backend knowledge.

---

## Core Team

The Core Team is responsible for:

- defining actions
- implementing business logic
- maintaining the Actions List
- testing action behavior
- preserving contract consistency

The Core Team owns the executable business behavior.

---

## API Team

The API Team is responsible for exposing actions through API endpoints.

The API Team should not duplicate business logic.

It maps API requests to actions.

---

## UI Team

The UI Team is responsible for building user interfaces on top of the action contract.

The UI Team can use the Actions List to understand:

- available operations
- required inputs
- expected outputs
- business constraints

The UI Team does not need to know the internal implementation of the backend.

---

## Summary

The core ADExMo vocabulary is simple:

```text
Domain -> Action -> Contract -> Runtime -> Adapter
```

A domain groups responsibilities.

An action defines executable business behavior.

A contract documents how the action is invoked and what it returns.

A runtime executes the action consistently.

An adapter connects an external interface to the action.

---

## Final Principle

> ADExMo defines what the system can execute.
> Implementation defines how the system executes it.
