# Core Concepts

This document defines the core concepts of ADExMo.

ADExMo, Action-Driven Execution Model, is based on a simple architectural principle:

> Business logic must be defined as executable actions, independent from any interface.

The purpose of these concepts is to create a shared vocabulary for analysis, development, testing, and integration.

---

## 1. Action

An **Action** is the fundamental execution unit in ADExMo.

An action represents a real business use case that the system can execute directly.

Example:

```pseudo
createUser(email: string, admin: bool = false)
```

An action must be:

- meaningful from a business perspective
- directly executable
- independent from HTTP, CLI, UI, or queue context
- defined by explicit inputs
- responsible for one clear outcome
- free from implementation details in its public definition

An action answers the question:

> What can the system do?

It does not answer:

> How is it technically implemented?

---

## 2. Domain

A **Domain** is a logical group of actions that represents a specific area of responsibility in the system.

Examples:

```text
user
order
invoice
environment
permission
```

A domain helps organize the Actions List and prevents the model from becoming a flat, unclear list of operations.

A valid domain must describe responsibility, not technology.

Valid domains:

```text
user
payment
booking
notification
```

Invalid domains:

```text
controller
database
api
repository
```

Every action must belong to exactly one domain.

A domain answers the question:

> Which area of responsibility does this action belong to?

---

## 3. Actions List

The **Actions List** is the official catalog of application behavior.

It defines the executable actions of the system and acts as the contract between analysis, development, API, UI, and integration teams.

The Actions List includes:

- domains
- action names
- intents
- inputs
- outputs
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

The Actions List answers the question:

> What behavior does the application officially provide?

---

## 4. Action Definition

An **Action Definition** is the detailed description of a single action.

It should define the action clearly enough that it can be implemented, tested, and integrated without guessing.

A complete Action Definition should include:

- name
- domain
- intent
- input
- output
- rules
- constraints
- signature

Example structure:

```markdown
# Action: order:process

## Intent

Process an existing order.

## Input

- orderId: int
- force: bool = false

## Output

- processed order
- error

## Rules

- the order must exist
- the order must not already be processed
- stock must be available unless force is true

## Constraints

- orderId must reference an existing order

## Signature

processOrder(orderId: int, force: bool = false)
```

An Action Definition answers the question:

> What exactly must this action do?

---

## 5. Action Signature

An **Action Signature** is the executable representation of an action.

It defines:

- action name
- input parameters
- parameter types
- default values

Example:

```pseudo
processOrder(orderId: int, force: bool = false)
```

The signature is important because it creates a direct bridge between analysis and implementation.

It must be clear, stable, and minimal.

A signature must not use generic containers unless strictly necessary.

Preferred:

```pseudo
createUser(email: string, admin: bool = false)
```

Avoid:

```pseudo
createUser(data: mixed)
createUser(request: Request)
```

An Action Signature answers the question:

> How is this action invoked?

---

## 6. Execution Layer

The **Execution Layer** is the part of the system responsible for executing actions.

It is placed between interface adapters and the action implementation.

Its typical responsibilities are:

- action resolution
- parameter binding
- validation
- context handling
- execution lifecycle
- result handling

Generic execution flow:

```text
Input
Resolution
Binding
Validation
Execution
Output
```

The Execution Layer ensures that actions are executed consistently, regardless of which interface invokes them.

It answers the question:

> How does the system execute actions in a uniform way?

---

## 7. Interface Adapter

An **Interface Adapter** connects an external interface to the Execution Layer.

Examples:

- HTTP adapter
- CLI adapter
- queue adapter
- test adapter
- integration adapter

An adapter must not contain business logic.

Its role is limited to:

1. receiving input
2. adapting input to action parameters
3. invoking the action
4. returning or forwarding the result

Example:

```pseudo
POST /users

runAction("user:create", {
    email: request.email,
    admin: request.admin
})
```

The controller does not create the user directly.

It invokes the action.

An Interface Adapter answers the question:

> How does a specific interface call the action?

---

## 8. Runtime

The **Runtime** is the technical mechanism that executes the ADExMo model.

Depending on the implementation, the runtime may provide:

- action registry
- metadata discovery
- parameter binding
- type conversion
- validation handling
- authentication context
- authorization context
- error handling
- logging hooks

The runtime is not the business logic.

It is the execution support around the business logic.

It answers the question:

> What technical mechanism runs the actions?

---

## 9. Action Registry

The **Action Registry** is the list of available executable actions known to the runtime.

It maps action identifiers to their executable implementation.

Example:

```text
user:create        -> createUser()
order:process      -> processOrder()
invoice:generate   -> generateInvoice()
```

The registry can be built manually or automatically, depending on the implementation.

Possible discovery mechanisms:

- explicit configuration
- annotations
- attributes
- decorators
- reflection
- framework registration

The Action Registry answers the question:

> Where does the runtime find the action to execute?

---

## 10. Input Contract

The **Input Contract** defines the data required to execute an action.

Inputs must be explicit, minimal, and meaningful.

Example:

```text
email: string
admin: bool = false
```

Avoid vague inputs:

```text
data: mixed
payload: array
request: Request
```

The input contract must not expose interface-specific structures.

For example, an action should not depend directly on an HTTP request object.

The Input Contract answers the question:

> What does this action need in order to run?

---

## 11. Output Contract

The **Output Contract** defines the expected result of an action.

The output can include:

- success result
- domain result
- validation error
- business error
- failure condition

Example:

```text
Output:
- created user
- validation error
- duplicate email error
```

The output contract should describe the result at the business level, not at the transport level.

Preferred:

```text
created user
```

Avoid:

```text
HTTP 201 response
```

HTTP status codes belong to the HTTP adapter, not to the action definition.

The Output Contract answers the question:

> What result does this action produce?

---

## 12. Business Rules

**Business Rules** define the conditions and behavior that the action must enforce.

Example:

```text
- a user email must be unique
- an order cannot be processed twice
- a payment can be refunded only if it was completed
```

Business rules belong inside the action behavior.

They must not be hidden inside controllers, CLI commands, or UI logic.

Business Rules answer the question:

> Which business conditions control this action?

---

## 13. Constraints

**Constraints** define limits, preconditions, or technical boundaries that affect the action.

Example:

```text
- orderId must reference an existing order
- the authenticated user must have permission to process the order
- the action must be idempotent
```

Constraints are not the same as implementation details.

They define conditions that must be respected for correct execution.

Constraints answer the question:

> Under which conditions can this action run correctly?

---

## 14. Context

**Context** represents execution information that may be required by the action but should not come from a specific interface.

Examples:

- authenticated user
- tenant
- locale
- correlation id
- permission scope

The context must be provided by the runtime or adapter in a controlled way.

The action should not directly depend on an HTTP request, CLI session, or UI state.

Context answers the question:

> Which execution information surrounds this action?

---

## 15. Contract Layer

The **Contract Layer** is the architectural role of the Actions List.

It defines what the system can do independently from how the system exposes it.

This contract can be consumed by:

- backend developers
- API teams
- frontend teams
- QA teams
- integration partners
- AI-assisted development workflows

The Contract Layer answers the question:

> What stable behavioral contract does the application expose?

---

## 16. Implementation

The **Implementation** is the internal code that performs the action.

ADExMo does not prescribe how the implementation must be written.

The implementation may use:

- Object-Oriented Programming
- Functional Programming
- procedural code
- domain services
- application services
- repositories
- external APIs
- framework services

ADExMo defines the execution interface, not the internal programming paradigm.

Implementation answers the question:

> How does the system perform the action internally?

---

## 17. FastCLI

**FastCLI** is a CLI adapter concept related to ADExMo.

It allows actions or functions to be exposed as CLI commands using metadata and function signatures.

FastCLI is not the core model.

It is one possible interface adapter.

ADExMo is broader:

```text
ADExMo = execution model
FastCLI = CLI adapter
```

FastCLI answers the question:

> How can actions be executed from the command line?

---

## 18. Summary Table

| Concept | Purpose |
|---|---|
| Action | Executable unit of business behavior |
| Domain | Logical group of actions by responsibility |
| Actions List | Versioned catalog of application behavior |
| Action Definition | Detailed description of one action |
| Action Signature | Executable invocation contract |
| Execution Layer | Uniform action execution path |
| Interface Adapter | Bridge between interface and action |
| Runtime | Technical mechanism that runs actions |
| Action Registry | Map of available executable actions |
| Input Contract | Explicit data required by an action |
| Output Contract | Expected result of an action |
| Business Rules | Business conditions enforced by the action |
| Constraints | Preconditions and boundaries |
| Context | Execution information around the action |
| Contract Layer | Stable behavioral contract of the application |
| Implementation | Internal code that performs the action |
| FastCLI | CLI adapter for executable actions |

---

## Final Principle

ADExMo separates three concerns:

```text
Action      -> what the system does
Adapter     -> how an interface invokes it
Runtime     -> how execution is managed
```

The action is the center of the model.

Everything else exists to invoke, execute, validate, or integrate it.

> One execution layer. Any interface.
