# From System Operations to Actions

## Purpose

This document explains how ADExMo Actions can be derived from application analysis.

It defines the relationship between:

- business processes
- use cases
- scenarios
- system operations
- system responsibilities
- action candidates
- validated ADExMo actions

The goal is to provide a clear bridge between functional analysis and executable business logic.

---

## The Problem

In many software projects, analysis and implementation are separated by an ambiguous gap.

Functional analysis usually describes:

- business processes
- actors
- use cases
- workflows
- requirements
- rules
- exceptions

Development teams must then interpret this material and decide:

- which services to create
- which controllers to write
- which API endpoints are needed
- where business logic should live
- which operations must be testable

This translation is often informal.

As a result, different teams may interpret the same analysis differently.

This leads to:

- duplicated logic
- unclear responsibility boundaries
- inconsistent API behavior
- UI-driven backend design
- late discovery of missing rules
- excessive clarification meetings
- rework during integration

ADExMo addresses this gap by introducing a formal transition from analysis to executable actions.

---

## Core Idea

An ADExMo Action usually derives from a system responsibility identified inside a use case flow.

A useful way to express this is:

> An ADExMo Action is the executable evolution of a System Operation.

This means that an action should not be invented directly from code structure.

It should be discovered from the behavior the system is responsible for executing.

---

## Analysis Hierarchy

A practical hierarchy can be represented as follows:

```text
Business Process
  ↓
Use Case
  ↓
Scenario / Flow Step
  ↓
System Operation
  ↓
Action Candidate
  ↓
Validated Action
  ↓
Action Definition
  ↓
Actions List
```

This hierarchy does not mean that every level must always be documented with the same level of detail.

It means that ADExMo Actions should be traceable back to real system behavior.

---

## Business Process

A Business Process describes a broader operational flow in the organization.

Examples:

- customer onboarding
- order fulfillment
- invoice management
- environment provisioning
- permission management

A Business Process is usually too broad to become one ADExMo Action.

It may contain multiple use cases.

---

## Use Case

A Use Case describes a goal that an actor wants to achieve by interacting with the system.

Examples:

- create a new customer account
- process an order
- generate an invoice
- create a development environment
- assign a permission to a user

A Use Case may produce one or more ADExMo Actions.

A Use Case is not always equal to one action.

---

## Scenario or Flow Step

A scenario describes the sequence of interactions between an actor and the system.

Example:

```text
Use Case:
Create a development environment

Flow:
1. The owner selects a project.
2. The owner requests a new environment.
3. The system checks permissions.
4. The system provisions the environment.
5. The system returns access information.
```

Not every flow step becomes an action.

Some steps are:

- user interactions
- UI behavior
- internal rules
- internal implementation details
- part of a larger action

The key question is:

> Where does the system take responsibility for producing a meaningful result?

---

## System Operation

A System Operation is an operation requested from the system by an actor or another system.

It represents a point where the system is asked to perform work.

Examples:

```text
createUser
processOrder
generateInvoice
createEnvironment
assignPermission
```

System Operation is a useful analysis concept because it is close to the ADExMo idea of an executable action.

However, ADExMo goes one step further.

A System Operation becomes an ADExMo Action only when it is:

- business-relevant
- independently executable
- clearly named
- assigned to a domain
- defined with explicit input and output
- free from implementation details
- valid according to the Action Definition Checklist

---

## System Responsibility

System Responsibility describes what the system is responsible for doing at a specific point in a flow.

It is often more useful than looking at user interface events.

Example:

```text
The user clicks "Create Environment".
```

This is not the action.

The system responsibility is:

```text
Create a development environment for the selected project.
```

The possible action is:

```text
environment:create
```

ADExMo Actions should be derived from system responsibilities, not from UI events.

---

## Action Candidate

An Action Candidate is a possible ADExMo Action discovered during analysis.

At this stage, it is not yet confirmed.

Example candidates:

```text
createEnvironment
checkEnvironmentPermission
provisionEnvironmentResources
returnEnvironmentAccess
```

Not all candidates become actions.

Some may be:

- internal rules
- implementation steps
- too technical
- too small
- part of a larger action
- not independently meaningful

---

## Validated Action

A Validated Action is an Action Candidate that passes the ADExMo validation rules.

A valid action must:

- represent a real business use case
- belong to exactly one domain
- have clear business intent
- have a single responsibility
- define explicit and minimal inputs
- produce a predictable output
- avoid implementation leakage
- be executable without depending on UI, HTTP, CLI or queue context

Only validated actions should enter the official Actions List.

---

## Action Definition

An Action Definition is the formal documentation of a validated action.

It includes:

- domain
- action name
- intent
- input
- output
- rules
- constraints
- signature

Example:

```markdown
## Action: environment:create

Domain:
environment

Intent:
Create a new development environment for a project.

Input:
- projectId: int
- ownerId: int
- environmentType: string

Output:
- created environment
- validation error
- authorization error

Rules:
- the owner must have permission to create environments
- the project must exist
- the environment type must be supported

Constraints:
- projectId must reference an existing project
- ownerId must reference an authorized user

Signature:
createEnvironment(projectId: int, ownerId: int, environmentType: string)
```

---

## Actions List

The Actions List is the collection of validated Action Definitions.

It is the official contract of application behavior.

The path from analysis to Actions List is therefore:

```text
Use Case
  ↓
System Responsibility
  ↓
Action Candidate
  ↓
Validated Action
  ↓
Action Definition
  ↓
Actions List
```

---

## One Use Case, One Action

Sometimes a use case maps cleanly to one action.

Example:

```text
Use Case:
Generate an invoice

Action:
invoice:generate
```

This is valid when the use case represents one clear system responsibility and one meaningful result.

---

## One Use Case, Multiple Actions

Often, a use case produces multiple actions.

Example:

```text
Use Case:
Manage user access
```

Possible actions:

```text
user:create
user:disable
permission:assign
permission:revoke
role:change
```

This is normal.

A use case may describe a larger goal, while actions represent executable responsibilities inside that goal.

---

## Multiple Use Cases, Shared Action

Sometimes different use cases invoke the same action.

Example:

```text
Use Case:
Admin creates a user

Use Case:
External onboarding imports a user

Shared Action:
user:create
```

This is also valid.

The same action can be reused by different interfaces, workflows or integrations if the business behavior is the same.

---

## What Should Not Become an Action

Not every step in an analysis flow should become an action.

Avoid turning the following into ADExMo Actions:

### UI Events

Invalid:

```text
clickSubmitButton
openModal
refreshTable
```

These are interface concerns.

### Technical Steps

Invalid:

```text
saveToDatabase
callExternalApi
fetchAndMapData
```

These are implementation details.

### Internal Sub-Steps

Invalid:

```text
checkStockAndThenSendEmail
validateInputAndPersistUser
```

These may be rules or internal implementation steps.

### Generic Operations

Invalid:

```text
manageData
handleProcess
executeOperation
```

These are too vague.

---

## Practical Discovery Questions

When reviewing a use case flow, ask:

1. Where does the system perform meaningful work?
2. Which step produces a business-relevant result?
3. Can this operation be executed without the UI?
4. Can it be tested independently?
5. Does it have clear input and output?
6. Does it belong to a clear domain?
7. Is it free from implementation details?
8. Would API, CLI, UI or queue adapters all be able to invoke it?

If the answer is yes, the operation may be an Action Candidate.

---

## Example: From Use Case to Actions

### Use Case

```text
Create a development environment
```

### Goal

Allow an authorized project owner to create a new development environment.

### Primary Actor

Project Owner

### Flow

| Step | Actor/System | Description | System Responsibility | Action Candidate |
|---|---|---|---|---|
| 1 | Actor | Selects a project | No | |
| 2 | Actor | Requests a new environment | Yes | createEnvironment |
| 3 | System | Checks whether the owner is authorized | Internal rule | |
| 4 | System | Provisions resources | Internal implementation | |
| 5 | System | Stores environment metadata | Internal implementation | |
| 6 | System | Returns access information | Output | |

### Validated Action

| Domain | Action | Signature |
|---|---|---|
| environment | environment:create | createEnvironment(projectId: int, ownerId: int, type: string) |

### Notes

The authorization check is not a separate action here because it is part of the rule set of `environment:create`.

The provisioning steps are not separate actions because they are implementation details inside the action.

The returned access information is output, not a separate action.

---

## Example: Splitting a Large Use Case

### Use Case

```text
Register and activate a customer account
```

Possible action candidates:

```text
createCustomer
verifyCustomerEmail
assignDefaultPlan
activateCustomerAccount
sendWelcomeEmail
```

Validated actions may be:

```text
customer:create
customer:verifyEmail
customer:activate
```

The other candidates may become:

- internal rules
- event handlers
- implementation steps
- asynchronous side effects

The decision depends on whether each candidate represents an independent business responsibility.

---

## Relationship with the Action Definition Checklist

This document explains where actions come from.

The Action Definition Checklist explains whether an action is valid.

The two documents should be used together.

A discovered action candidate must pass the checklist before it becomes part of the official Actions List.

---

## Relationship with Contract-Based Development

After actions are validated and documented, they become part of the Actions List.

The Actions List can then be used as a development contract for:

- backend implementation
- API mapping
- UI development
- CLI execution
- testing
- external integrations

This creates a clear bridge:

```text
Analysis
  ↓
Actions List
  ↓
Contract-Based Development
```

---

## Role in the ADExMo Workflow

The transformation from analysis to actions is a key ADExMo activity.

A practical workflow is:

1. Review business process analysis.
2. Identify use cases.
3. Analyze scenario flows.
4. Identify system responsibilities.
5. Extract action candidates.
6. Validate candidates using the checklist.
7. Assign each action to a domain.
8. Define input, output, rules and constraints.
9. Produce the Actions List.
10. Use the Actions List as the execution contract.

---

## Common Mistake: Deriving Actions from Screens

A frequent mistake is to derive actions from UI screens.

Example:

```text
Screen:
User management page
```

Poor action candidates:

```text
loadUserPage
clickCreateUser
openEditUserModal
saveUserForm
```

Better action candidates:

```text
user:create
user:update
user:disable
permission:assign
```

Actions should describe system behavior, not screen behavior.

---

## Common Mistake: Deriving Actions from Database Tables

Another mistake is to derive actions directly from database entities.

Example:

```text
Table:
users
```

Poor action candidates:

```text
insertUser
updateUserRow
deleteUserRecord
```

Better action candidates:

```text
user:create
user:updateProfile
user:disable
user:authenticate
```

The database is implementation.

The action is behavior.

---

## Common Mistake: Deriving Actions from API Endpoints

API endpoints are adapters.

They should not be the source of business logic.

Poor approach:

```text
POST /users -> defines the business behavior
```

Better approach:

```text
Action:
user:create

HTTP Adapter:
POST /users -> user:create
```

In ADExMo, the action exists before the API endpoint.

---

## Summary

ADExMo Actions should be derived from analysis, but not directly from every analysis artifact.

The most useful bridge is the System Operation or System Responsibility.

The practical transformation is:

```text
Use Case
  ↓
System Responsibility
  ↓
Action Candidate
  ↓
Validated Action
  ↓
Action Definition
  ↓
Actions List
```

A use case may produce one action, multiple actions or reuse existing actions.

The key rule is simple:

> An ADExMo Action begins where the system takes responsibility for producing a meaningful, executable business result.

---

## Final Principle

> ADExMo does not replace functional analysis.
> It makes functional analysis executable.
