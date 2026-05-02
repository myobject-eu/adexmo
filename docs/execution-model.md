# ADExMo Execution Model

## Purpose

This document defines the execution model of ADExMo.

ADExMo is based on a simple architectural principle:

> Business logic must be executed through actions, not through interfaces.

The execution model describes how an input coming from any interface is resolved, validated, bound to an action, executed, and returned as output.

---

## Core Principle

In ADExMo, the Service Layer is the single execution point of the application.

Interfaces do not contain business logic.

They only invoke executable actions.

Examples of interfaces:

- HTTP controllers
- CLI commands
- background jobs
- scheduled tasks
- test runners
- integration adapters

Each of these interfaces may receive input differently, but they must all invoke the same action when the business operation is the same.

---

## Execution Unit

The fundamental execution unit in ADExMo is the **Action**.

An action represents a real use case of the system.

Example:

```pseudo
createUser(email: string, admin: bool = false)
```

This action can be invoked by:

```text
HTTP Controller  -> createUser
CLI Command      -> createUser
Queue Job        -> createUser
Test Runner      -> createUser
```

The execution path remains the same.

Only the adapter changes.

---

## High-Level Execution Flow

The ADExMo execution flow is composed of six main steps:

```text
Input
Resolution
Binding
Validation
Execution
Output
```

Each step has a specific responsibility.

---

## 1. Input

Input is the raw data received from an external or internal trigger.

Examples:

### HTTP Input

```json
{
  "email": "user@example.com",
  "admin": true
}
```

### CLI Input

```bash
app user:create user@example.com --admin
```

### Queue Input

```json
{
  "action": "user:create",
  "payload": {
    "email": "user@example.com",
    "admin": true
  }
}
```

The input format may change depending on the interface.

The action contract must not change.

---

## 2. Resolution

Resolution identifies which action must be executed.

The runtime receives an action identifier and resolves it to a concrete executable action.

Example:

```text
user:create -> createUser(email: string, admin: bool = false)
```

Resolution may be based on:

- action name
- domain and action pair
- metadata
- attributes
- decorators
- configuration
- registry lookup

The exact implementation depends on the language or framework.

The architectural rule remains the same:

> The action must be resolved before execution, independently from the interface that triggered it.

---

## 3. Binding

Binding maps input values to action parameters.

Example action:

```pseudo
createUser(email: string, admin: bool = false)
```

Input:

```json
{
  "email": "user@example.com",
  "admin": true
}
```

Bound parameters:

```text
email = "user@example.com"
admin = true
```

Binding is responsible for:

- matching input fields to parameters
- applying default values
- converting basic types
- detecting missing required values
- rejecting unknown or invalid parameters when necessary

---

## 4. Validation

Validation ensures that the bound input satisfies the action contract.

Validation may include:

- required parameter checks
- type checks
- format checks
- business preconditions
- permission checks
- context checks

Example:

```text
email must be a valid email address
admin must be a boolean value
```

Validation belongs to the execution model, but business validation belongs to the action or to the domain logic behind the action.

The adapter must not become the owner of business validation.

---

## 5. Execution

Execution invokes the resolved action with the validated and bound parameters.

Example:

```pseudo
result = createUser(
    email: "user@example.com",
    admin: true
)
```

At this point, the interface is no longer relevant.

The action executes the business operation.

The same action must behave consistently regardless of whether it was invoked from:

- HTTP
- CLI
- queue
- tests
- internal runtime

---

## 6. Output

Output is the result produced by the action.

The action should return a clear and predictable result.

Example:

```json
{
  "status": "success",
  "userId": 123
}
```

The adapter is responsible for transforming the result into the format required by the interface.

Examples:

```text
HTTP Adapter -> JSON response
CLI Adapter  -> console output
Queue Adapter -> job result or event
Test Runner  -> assertion value
```

The business result remains the same.

Only the presentation changes.

---

## Runtime Responsibility

The ADExMo runtime is the layer responsible for managing action execution.

Typical runtime responsibilities:

- action registration
- action resolution
- parameter binding
- basic type conversion
- validation orchestration
- context handling
- execution lifecycle
- error normalization
- output forwarding

The runtime does not replace the business logic.

It coordinates execution.

---

## Adapter Responsibility

An adapter connects an interface to the execution runtime.

An adapter may be:

- an HTTP adapter
- a CLI adapter
- a queue adapter
- a scheduler adapter
- a test adapter
- an integration adapter

Adapters are responsible for:

1. receiving input
2. extracting parameters
3. calling the runtime
4. returning the result in the interface-specific format

Adapters are not responsible for:

- business decisions
- business workflows
- domain rules
- internal implementation logic

---

## Action Responsibility

An action is responsible for executing a business use case.

An action must:

- represent a clear business intent
- receive explicit input parameters
- execute independently from any interface
- return a predictable result
- preserve the system responsibility boundary

An action must not depend on:

- HTTP request objects
- controller state
- CLI-specific state
- UI-specific state
- framework-specific interface context

If an action requires an interface-specific object to run, it is not a valid ADExMo action.

---

## Context Handling

Some executions require context.

Examples:

- authenticated user
- tenant
- organization
- locale
- permissions
- execution mode
- correlation ID

In ADExMo, context must be handled explicitly by the runtime or passed through a controlled execution context.

The action should not secretly depend on the interface that created the context.

Valid approach:

```pseudo
runAction("invoice:generate", params, context)
```

Invalid approach:

```pseudo
invoiceAction reads directly from HTTP request session
```

The action may use context, but the context must be provided through the execution model, not pulled from an interface-specific global state.

---

## Error Handling

Errors should be normalized by the execution model.

Common error categories:

- action not found
- invalid input
- missing required parameter
- validation error
- permission denied
- business rule violation
- execution failure

Adapters may transform these errors into interface-specific responses.

Examples:

```text
HTTP Adapter -> 400, 403, 404, 500
CLI Adapter  -> error message and exit code
Queue Adapter -> failed job or retry
```

The error meaning must remain consistent across interfaces.

---

## Before ADExMo

Traditional application structure often looks like this:

```text
HTTP Controller -> Service
CLI Command     -> Service
Queue Job       -> Service
```

In practice, each entry point may introduce:

- different validation
- different mapping
- different assumptions
- different error handling
- duplicated behavior

This creates multiple execution paths for the same business operation.

---

## With ADExMo

ADExMo simplifies the execution model:

```text
HTTP Controller -> Adapter -> Runtime -> Action
CLI Command     -> Adapter -> Runtime -> Action
Queue Job       -> Adapter -> Runtime -> Action
Test Runner     -> Adapter -> Runtime -> Action
```

The action remains the same.

The execution path is unified.

---

## Example

### Action Definition

```pseudo
user:create

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

### HTTP Adapter

```pseudo
POST /users

runAction("user:create", {
    email: request.email,
    admin: request.admin
})
```

### CLI Adapter

```pseudo
app user:create user@example.com --admin

runAction("user:create", {
    email: "user@example.com",
    admin: true
})
```

### Queue Adapter

```pseudo
runAction("user:create", job.payload)
```

All adapters invoke the same action.

---

## Execution Contract

The execution contract defines what is required to execute an action.

At minimum, it includes:

- action identifier
- input parameters
- expected output
- rules
- constraints
- execution context, when needed

Example:

```text
Action: user:create
Input: email, admin
Output: created user or validation error
Context: authenticated operator, optional
```

The contract is documented in the Actions List.

---

## Testing Impact

ADExMo makes business logic testable before any interface exists.

A test can invoke the action directly:

```pseudo
result = runAction("user:create", {
    email: "user@example.com",
    admin: false
})

assert result.status == "success"
```

This avoids waiting for:

- controllers
- API endpoints
- UI screens
- CLI commands
- queue workers

The business behavior can be validated from day one.

---

## Team Impact

ADExMo allows teams to work in parallel.

### Core Team

Responsible for:

- defining actions
- implementing business logic
- validating execution
- maintaining the Actions List

### API Team

Responsible for:

- exposing actions through HTTP
- mapping routes to actions
- formatting responses

### UI Team

Responsible for:

- building user interaction
- calling API endpoints or integration adapters
- presenting results

### Integration Team

Responsible for:

- invoking actions from external systems
- mapping external data to action input contracts

The Actions List becomes the shared contract.

---

## Design Rules

The execution model follows these rules:

1. Business logic belongs to actions.
2. Interfaces invoke actions.
3. Adapters transform input and output.
4. The runtime coordinates execution.
5. Actions must run without interface context.
6. Validation must not be duplicated across interfaces.
7. The same business operation must have one execution path.
8. The Actions List is the reference contract.

---

## Invalid Patterns

### Business Logic in Controller

```pseudo
POST /users

if user exists:
    return error

create user
send email
assign role
```

Problem:

The HTTP controller owns business behavior.

---

### Business Logic in CLI Command

```pseudo
app user:create

validate user
create user
assign role
```

Problem:

The CLI command duplicates behavior that may already exist elsewhere.

---

### Interface-Specific Action

```pseudo
createUserFromHttpRequest(request)
```

Problem:

The action depends on HTTP context.

---

## Valid Pattern

```pseudo
createUser(email: string, admin: bool = false)
```

Then:

```pseudo
HTTP -> Adapter -> createUser
CLI  -> Adapter -> createUser
Job  -> Adapter -> createUser
Test -> Adapter -> createUser
```

The business operation has one owner.

---

## Relationship with the Actions List

The execution model depends on the Actions List.

The Actions List defines:

- what actions exist
- which domain they belong to
- what input they require
- what output they return
- which rules apply
- which constraints must be respected

The runtime executes what the Actions List defines.

The implementation must not invent hidden actions outside the documented contract.

---

## Summary

ADExMo defines a unified execution model for application behavior.

The model separates:

```text
what the system does
```

from:

```text
how the system is triggered
```

The result is a cleaner architecture where business logic is:

- directly executable
- easier to test
- independent from interfaces
- consistent across entry points
- easier to integrate
- easier to document

Core principle:

> One execution layer. Any interface.
