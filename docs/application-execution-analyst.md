# Application Execution Analyst

## Purpose

This document defines the role of the Application Execution Analyst in the ADExMo workflow.

The Application Execution Analyst is the professional role responsible for transforming functional analysis into a validated Actions List.

This role operates between analysis and development.

Its main responsibility is to convert use cases, system responsibilities and business rules into executable action contracts.

---

## Core Definition

An Application Execution Analyst is responsible for identifying, validating and documenting the executable business actions of an application.

The role focuses on the transition from:

```text
functional analysis
```

to:

```text
validated executable actions
```

The main deliverable of this role is the ADExMo Actions List.

---

## Why This Role Matters

In many software projects, the transition between analysis and development is weak.

Business analysts describe processes and requirements.

Developers interpret those requirements and implement code.

Between these two activities, there is often an informal and ambiguous translation step.

This creates common problems:

- unclear system responsibilities
- duplicated business logic
- excessive dependency on developers' interpretation
- incomplete API contracts
- UI-driven backend behavior
- inconsistent implementation across interfaces
- rework during integration
- frequent clarification meetings
- late discovery of missing rules

ADExMo makes this transition explicit.

The Application Execution Analyst owns that transition.

---

## Position in the Workflow

The role sits between process analysis and software implementation.

```text
Business / Process Analysis
        ↓
Functional Analysis
        ↓
Application Execution Analyst
        ↓
Actions List
        ↓
Backend / API / UI Development
```

The Application Execution Analyst does not replace business analysts or developers.

The role connects them through a precise execution contract.

---

## Primary Responsibility

The primary responsibility of the Application Execution Analyst is to answer this question:

> What must the system be able to execute?

This is different from asking:

```text
Which screens do we need?
Which endpoints do we need?
Which classes do we need?
Which database tables do we need?
```

Those questions belong to later design and implementation phases.

The Application Execution Analyst focuses on executable business behavior.

---

## Main Deliverables

The Application Execution Analyst may produce or maintain:

- Domain Map
- Use Case to Actions Mapping
- Action Candidates
- Validated Actions
- Action Definitions
- Actions List
- Action signatures
- input and output contracts
- business rules linked to actions
- constraints linked to actions
- notes for API and UI teams
- validation notes
- contract change notes

The most important deliverable is the Actions List.

---

## Main Activities

### 1. Review Functional Analysis

The Application Execution Analyst reviews:

- business processes
- use cases
- actors
- workflows
- requirements
- exceptions
- business rules
- constraints

The goal is not to rewrite the analysis.

The goal is to extract executable system behavior from it.

---

### 2. Identify System Responsibilities

The analyst identifies where the system takes responsibility for doing meaningful work.

Example:

```text
The user clicks the Create Environment button.
```

This is not the action.

The system responsibility is:

```text
Create a development environment for the selected project.
```

Possible action:

```text
environment:create
```

---

### 3. Extract Action Candidates

The analyst extracts possible actions from use case flows and system responsibilities.

At this stage, they are only candidates.

Example:

```text
createEnvironment
checkEnvironmentPermission
provisionEnvironmentResources
returnEnvironmentAccess
```

Not every candidate becomes an action.

Some are rules, outputs or implementation details.

---

### 4. Validate Action Candidates

Each candidate must be validated against ADExMo rules.

A valid action must:

- represent a real business use case
- belong to exactly one domain
- have clear business intent
- have a single responsibility
- define explicit and minimal inputs
- produce a predictable output
- avoid implementation leakage
- be executable independently from UI, HTTP, CLI or queue context

Invalid candidates are revised, merged, split or rejected.

---

### 5. Define Domains

The Application Execution Analyst organizes actions into domains.

Domains must represent responsibilities, not technical components.

Valid domains:

```text
user
order
invoice
environment
permission
```

Invalid domains:

```text
controller
database
api
repository
```

Domains give structure to the Actions List.

---

### 6. Write Action Definitions

For each validated action, the analyst defines:

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
- type: string

Output:
- created environment
- validation error
- authorization error

Rules:
- the owner must be authorized
- the project must exist
- the environment type must be supported

Signature:
createEnvironment(projectId: int, ownerId: int, type: string)
```

---

### 7. Support Development Teams

The Application Execution Analyst helps backend, API and UI teams understand the Actions List.

The analyst clarifies:

- what each action does
- which inputs are required
- which outputs are expected
- which rules belong to the backend action
- which behavior belongs to adapters
- which behavior belongs to UI only
- which parts are implementation details

This reduces ambiguity between teams.

---

### 8. Maintain Contract Consistency

As the project evolves, the analyst helps preserve consistency in the Actions List.

Typical responsibilities include:

- detecting duplicate actions
- identifying inconsistent naming
- preventing technical domains
- avoiding implementation leakage
- reviewing breaking changes
- keeping input/output contracts clear
- ensuring new actions fit existing domains

---

## Required Skills

The Application Execution Analyst needs hybrid skills.

This role is not purely functional and not purely technical.

---

## Functional Analysis Skills

The role requires the ability to understand:

- business processes
- use cases
- actors
- workflows
- operational goals
- business rules
- exceptions
- constraints
- organizational responsibilities

The analyst must be able to read functional analysis and extract system behavior from it.

---

## Programming Awareness

The role requires programming awareness.

The analyst does not necessarily need to be the strongest developer on the team, but must understand:

- functions
- parameters
- input and output
- data types
- validation
- execution flow
- single responsibility
- testability
- separation of concerns
- interface independence

Without this technical awareness, the Actions List risks becoming vague functional documentation instead of an executable contract.

---

## Architectural Awareness

The role must understand basic architectural boundaries.

In particular:

- UI is not business logic
- API is an adapter
- CLI is an adapter
- queue jobs are adapters
- controllers should not own business rules
- actions are execution units
- domains are responsibility groups
- implementation details must stay inside actions

This awareness is essential to preserve the ADExMo model.

---

## Communication Skills

The Application Execution Analyst must communicate with:

- business analysts
- process analysts
- developers
- API teams
- UI teams
- testers
- project managers
- external suppliers

The role requires precision.

Ambiguous language produces ambiguous actions.

---

## Relationship with Other Roles

## Business Analyst

A Business Analyst focuses on business needs, requirements and processes.

The Application Execution Analyst uses this material to define executable system behavior.

The Business Analyst asks:

```text
What does the business need?
```

The Application Execution Analyst asks:

```text
What must the system execute to satisfy that need?
```

---

## Functional Analyst

A Functional Analyst describes system behavior from a functional perspective.

The Application Execution Analyst refines that behavior into validated actions, signatures and contracts.

The roles may overlap, especially in smaller teams.

ADExMo gives this overlap a more precise structure.

---

## Requirements Engineer

A Requirements Engineer defines and manages requirements.

The Application Execution Analyst transforms relevant requirements into executable action contracts.

The two roles are compatible.

The difference is that ADExMo focuses on the execution boundary.

---

## Systems Analyst

A Systems Analyst studies how the system should behave.

This is close to the Application Execution Analyst.

The difference is that the Application Execution Analyst has a specific deliverable:

```text
the Actions List
```

---

## Solution Architect

A Solution Architect defines the overall technical solution.

The Application Execution Analyst defines executable business actions.

The architect may decide technologies, integration strategy and infrastructure.

The Application Execution Analyst ensures that the business logic contract remains clear.

---

## Developer

A Developer implements actions and adapters.

The Application Execution Analyst defines what must be implemented as executable behavior.

The developer decides how to implement it internally, while preserving the action contract.

---

## API Developer

An API Developer exposes actions through HTTP or other API protocols.

The API Developer should map endpoints to actions.

The API should not become the source of business logic.

---

## UI Developer

A UI Developer builds screens and workflows based on the Actions List.

The UI Developer does not need to know the internal implementation of the backend.

The UI should call or consume adapters that invoke actions.

---

## Tester

A Tester can use the Actions List as a test planning artifact.

Each action can be tested independently.

This improves test coverage before full UI or API integration is complete.

---

## External Supplier

An external supplier may develop UI, API or integration layers using the Actions List as a contract.

This is one of the strongest practical advantages of ADExMo.

The supplier does not need full internal backend knowledge.

They need a clear contract.

---

## What This Role Is Not

The Application Execution Analyst is not:

- a generic project manager
- a UI designer
- a database designer
- a framework specialist
- a replacement for developers
- a replacement for business analysts
- a person who writes vague functional documentation

The role is focused on executable business behavior.

---

## Typical Workflow

A practical workflow for the Application Execution Analyst is:

1. Read business process documentation.
2. Identify use cases.
3. Analyze use case flows.
4. Identify system responsibilities.
5. Extract action candidates.
6. Reject UI events and implementation details.
7. Validate candidates using the Action Definition Checklist.
8. Assign each validated action to a domain.
9. Define action input, output, rules and constraints.
10. Write the action signature.
11. Add the action to the Actions List.
12. Review the Actions List with backend, API and UI teams.
13. Maintain consistency as the project evolves.

---

## Example

### Functional Statement

```text
A project owner can create a new development environment for a project.
The system must check whether the owner is authorized.
If authorized, the system provisions the environment and returns access information.
```

### Analysis

System responsibility:

```text
Create a development environment for a selected project.
```

Action candidate:

```text
createEnvironment
```

Validated action:

```text
environment:create
```

Signature:

```pseudo
createEnvironment(projectId: int, ownerId: int, type: string)
```

Rules:

```text
- the owner must be authorized
- the project must exist
- the environment type must be supported
```

Output:

```text
- created environment
- authorization error
- validation error
```

The authorization check is not necessarily a separate action.

It may be a rule inside `environment:create`.

The provisioning steps are implementation details.

The access information is output.

---

## Common Mistakes

## Mistake 1: Turning UI Events into Actions

Invalid:

```text
clickCreateButton
openUserModal
submitForm
```

Valid:

```text
user:create
environment:create
```

Actions describe system behavior, not UI events.

---

## Mistake 2: Turning Technical Steps into Actions

Invalid:

```text
saveToDatabase
callExternalApi
mapJsonPayload
```

Valid:

```text
invoice:generate
order:process
```

Actions describe business behavior, not implementation details.

---

## Mistake 3: Creating Generic Actions

Invalid:

```text
handleData
manageUser
executeProcess
```

Valid:

```text
user:create
user:disable
permission:assign
```

Actions must be specific.

---

## Mistake 4: Ignoring Domains

An action without a clear domain is usually not ready.

Domains prevent the Actions List from becoming a flat and confused list.

---

## Mistake 5: Accepting Vague Inputs

Invalid:

```pseudo
createUser(data: mixed)
```

Better:

```pseudo
createUser(email: string, role: string)
```

Inputs must be explicit and minimal.

---

## Value for Organizations

The Application Execution Analyst creates value by:

- reducing ambiguity between analysis and development
- improving communication between teams
- making business behavior testable earlier
- supporting external UI and API development
- reducing rework
- improving onboarding
- preserving architectural clarity
- making the Actions List a reliable contract

This role is especially useful in projects with:

- multiple teams
- external suppliers
- backend-heavy systems
- complex business rules
- long-term maintenance needs
- multiple interfaces over the same logic
- API and UI developed by different teams

---

## Is This a New Role?

The role is not completely new in the market.

It overlaps with existing roles such as:

- Functional Analyst
- Technical Analyst
- Systems Analyst
- Requirements Engineer
- Solution Analyst
- Business Logic Analyst

However, ADExMo gives this responsibility a more precise focus.

The distinctive responsibility is:

> transforming analysis artifacts into a validated Actions List that can be used as an executable development contract.

In this sense, ADExMo does not necessarily invent a new profession from nothing.

It formalizes a responsibility that often already exists informally.

---

## Possible Role Names

Possible names include:

- Application Execution Analyst
- ADExMo Action Analyst
- Action Definition Specialist
- Business Logic Analyst
- Technical Functional Analyst
- Execution Analyst

The most neutral and descriptive name is:

```text
Application Execution Analyst
```

The most ADExMo-specific name is:

```text
ADExMo Action Analyst
```

---

## Relationship with ADExMo Documentation

This role is closely related to the following documents:

- `from-system-operations-to-actions.md`
- `actions-list.md`
- `domains.md`
- `action-definition-checklist.md`
- `contract-based-development.md`

Together, these documents define:

```text
how actions are discovered
how actions are validated
how actions are organized
how actions become a contract
who can govern this transformation
```

---

## Summary

The Application Execution Analyst is the role responsible for turning analysis into executable structure.

The role connects business understanding with technical execution.

Its main output is the Actions List.

The role ensures that application behavior is:

- clear
- structured
- testable
- interface-independent
- ready for implementation
- usable by backend, API, UI and external teams

---

## Final Principle

> The Application Execution Analyst does not simply document what the business wants.
> The role defines what the system must be able to execute.
