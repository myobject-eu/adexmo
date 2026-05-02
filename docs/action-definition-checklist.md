# ADExMo Action Definition Checklist

## Purpose

This checklist defines the mandatory criteria that every ADExMo action must satisfy.

It acts as a guardrail to ensure that actions:

- represent real business intent
- remain independent from implementation details
- are directly executable
- preserve architectural boundaries
- can be used as a stable contract by API, UI, CLI and integration teams

If an action fails any of these checks, it must be revised before implementation.

---

## Why This Checklist Matters

A valid ADExMo action is not only a unit of backend logic.

It is also a contract.

A well-defined action can be used by other teams without requiring knowledge of the internal implementation. API teams, UI teams, CLI adapters, queue workers and external integrators can all rely on the same action definition.

For this reason, every action must be clear, stable, executable and free from implementation leakage.

The full organizational role of the Actions List as a development contract is documented separately in:

```text
contract-based-development.md
```

This checklist focuses only on whether an action is properly defined.

---

## Rule 0: Domain Grouping

**Actions must be organized into domains before their definition.**

Domains represent the high-level responsibilities of the system and serve to:

- structure the Actions List
- prevent duplication and ambiguity
- improve readability and navigation
- support collaboration across teams
- keep actions grouped by responsibility rather than by technical layer

A domain is a logical group of actions that belong to the same area of responsibility.

Valid examples:

```text
user        -> identity and authentication management
environment -> environment lifecycle management
permission  -> authorization and access control
invoice     -> invoice generation and lifecycle management
```

Invalid examples:

```text
database
api
controller
repository
service
```

Domains must not reflect technical implementation details.

Rules:

- every action must belong to exactly one domain
- domains must be defined before actions
- domains must represent responsibilities, not data structures or technical components
- a domain must be understandable without knowing the internal codebase

If a clear domain does not exist, the action is not yet properly defined.

---

## Rule 1: Business Intent

**The action must represent a real use case.**

Ask:

- Does this action describe something meaningful from a business perspective?
- Can it be understood without technical context?
- Does it describe a behavior the system is expected to provide?

Valid examples:

```text
createUser
processOrder
generateInvoice
assignPermission
createEnvironment
```

Invalid examples:

```text
handleData
executeProcess
manageEntity
performOperation
runLogic
```

An action must express business meaning, not generic execution.

---

## Rule 2: Single Responsibility

**The action must represent one responsibility only.**

Ask:

- Does the action describe a single outcome?
- Is it free from combined responsibilities?
- Would splitting it improve clarity?

Valid examples:

```text
processOrder
generateInvoice
archiveEnvironment
```

Invalid examples:

```text
checkStockAndProcessOrder
validateAndSaveUser
createUserAndSendWelcomeEmail
calculateAndPersistReport
```

If multiple responsibilities are present, split the action.

An action may internally perform several technical steps, but externally it must represent one clear business outcome.

---

## Rule 3: No Implementation Leakage

**The action must not expose implementation details.**

Ask:

- Does the name include technical steps?
- Does it reveal how the system works internally?
- Does it mention storage, APIs, controllers, repositories or internal services?

Valid examples:

```text
generateReport
createUser
processPayment
syncCustomer
```

Invalid examples:

```text
fetchDataAndFormatAndSaveReport
callApiAndPersistResult
insertUserIntoDatabase
updateRepositoryRecord
sendHttpRequestToProvider
```

Implementation must remain inside the action, not in its definition.

The action defines what the system does, not how it does it.

---

## Rule 4: Executable in Isolation

**The action must be directly executable without any interface.**

Ask:

- Can this action be executed from CLI, test or runtime directly?
- Does it depend on UI, controller, request or session state?
- Can it be called by different adapters without changing its behavior?

Valid example:

```pseudo
runAction('order:process', {
    orderId: 123,
    force: false
})
```

Invalid cases:

```text
requires HTTP request context
depends on controller state
depends on a UI form object
depends on route-specific behavior
depends on frontend state
```

If it cannot run independently, it is not an ADExMo action.

---

## Rule 5: Clear Input Contract

**The action must define explicit and minimal inputs.**

Ask:

- Are all required inputs clearly defined?
- Are the input types explicit?
- Are there unnecessary or ambiguous parameters?
- Is the action avoiding generic containers?

Valid examples:

```pseudo
processOrder(orderId: int, force: bool = false)
createUser(email: string, admin: bool = false)
assignPermission(userId: int, permissionCode: string)
```

Invalid examples:

```pseudo
processOrder(data: mixed)
createUser(requestObject)
assignPermission(payload: array)
handleInput(formData)
```

Inputs must be precise, not generic containers.

Generic payloads hide the contract and make the action harder to validate, test and integrate.

---

## Rule 6: Clear Output Contract

**The action must produce a predictable and documented result.**

Ask:

- Is the expected output clear?
- Are success and failure cases defined?
- Can API and UI teams understand what they will receive?
- Are errors part of the expected contract?

Valid examples:

```text
created user
confirmed order
validation error
permission denied
not found error
```

Invalid examples:

```text
returns something
returns mixed result
throws unknown error
output depends on caller
```

The output does not need to expose internal data structures, but it must be predictable enough for integration teams to use.

---

## Rule 7: System Responsibility Boundary

**The action must begin where the system takes responsibility.**

Ask:

- Is this the point where the system performs work?
- Is it triggered by an external actor or another system?
- Is it an entry point into application behavior?

Valid examples:

```text
User -> System: createOrder -> Action
External System -> System: importCustomer -> Action
Scheduler -> System: generateMonthlyInvoices -> Action
```

Invalid examples:

```text
User -> User interaction
internal service-to-service call
private helper method
format internal array
calculate temporary value
```

Only meaningful entry points into application behavior are valid actions.

Internal helper methods are not ADExMo actions.

---

## Rule 8: Naming Clarity

**The action name must follow a clear verb + object structure.**

Ask:

- Is the name simple and direct?
- Can it be understood without context?
- Does it describe the result of the action?

Valid examples:

```text
createUser
processPayment
generateInvoice
archiveEnvironment
assignPermission
```

Invalid examples:

```text
doStuff
handleEverything
executeOperation
manageSystem
processData
```

The name must describe a business operation, not a vague activity.

---

## Rule 9: No Logical Ambiguity

**The action must produce a predictable outcome.**

Ask:

- Is the result of the action clear?
- Are rules and constraints well defined?
- Are side effects intentional and documented?
- Would two developers understand the same behavior from the definition?

Valid examples:

```text
returns success or validation error
creates one user account
archives one environment
assigns one permission to one user
```

Invalid examples:

```text
unclear behavior
hidden side effects
changes different things depending on caller
combines unrelated outcomes
```

Ambiguity in an action definition becomes ambiguity in implementation, testing and integration.

---

## Rule 10: Interface Independence

**The action must not be designed for one specific interface.**

Ask:

- Can the action be invoked from HTTP, CLI, queue or tests?
- Does it avoid HTTP-specific or UI-specific concepts?
- Does it avoid depending on route names, form names or page state?

Valid examples:

```pseudo
createUser(email: string, admin: bool = false)
generateInvoice(orderId: int)
```

Invalid examples:

```pseudo
createUserFromHttpRequest(request)
generateInvoiceFromButtonClick(formState)
processOrderFromController(controllerContext)
```

The same action must be usable by multiple adapters.

Interfaces invoke actions. They do not define them.

---

## Rule 11: Stable Contract for Other Teams

**The action must be clear enough to be consumed by teams that do not know the internal implementation.**

Ask:

- Can a UI team understand what screen or form is needed?
- Can an API team map an endpoint to this action?
- Can a CLI adapter expose this action for testing?
- Can an external partner understand the expected input and output?

A valid action definition should allow other teams to work from the contract alone.

This does not mean every implementation detail must be documented.

It means the action contract must be sufficient for integration.

---

## Quick Validation Test

Before accepting an action, verify:

1. Can I explain it in one sentence?
2. Is it a real business use case?
3. Does it belong to a clear domain?
4. Does it avoid implementation details?
5. Can it run without any interface?
6. Are inputs explicit and minimal?
7. Is the output predictable?
8. Can API, UI or CLI teams use it as a contract?

If any answer is no, the action is not valid yet.

---

## Minimum Action Definition

Every accepted action should define at least:

```markdown
### Action: <domain>:<action>

Intent:
<short description of the expected business result>

Input:
- <name>: <type> <required/optional> <description>

Output:
- <success result>
- <possible error result>

Rules:
- <business rule>

Constraints:
- <constraint>

Signature:
<functionName>(param: type)
```

---

## Example of a Valid Action

```markdown
### Action: user:create

Intent:
Create a new user account.

Input:
- email: string, required, user email address
- admin: bool, optional, default false

Output:
- created user
- validation error

Rules:
- email must be unique
- admin users can only be created by authorized operators

Constraints:
- email must be valid
- caller must have permission to create users

Signature:
createUser(email: string, admin: bool = false)
```

Why it is valid:

- it belongs to a clear domain
- it represents a real use case
- it has one responsibility
- it has explicit inputs
- it has predictable outputs
- it does not expose implementation details
- it can be executed from HTTP, CLI, tests or queue adapters

---

## Example of an Invalid Action

```markdown
### Action: api:saveUserRequest

Intent:
Handle the user creation request from the API and save it into the database.

Input:
- request: Request

Output:
- mixed response

Signature:
saveUserRequest(request: Request)
```

Why it is invalid:

- `api` is a technical domain
- the action depends on HTTP request context
- the input contract is unclear
- the name exposes implementation details
- the output is ambiguous
- it is not interface-independent

A better version would be:

```markdown
### Action: user:create

Signature:
createUser(email: string, admin: bool = false)
```

---

## Core Principle

> If the action exposes implementation, it is not an action.

---

## Summary

An ADExMo action must be:

- meaningful
- domain-based
- atomic
- executable
- interface-independent
- explicit in input
- predictable in output
- free from implementation leakage
- usable as a contract by other teams

This checklist ensures that every action remains aligned with the ADExMo model and preserves the integrity of the execution layer.
