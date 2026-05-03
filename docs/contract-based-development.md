# Contract-Based Development with ADExMo

## Purpose

This document explains how the ADExMo Actions List can be used as a development contract between teams.

The goal is to make application behavior clear, testable and usable by API, UI and integration teams without requiring them to know the internal implementation of the business logic.

ADExMo does not only define how actions are executed.

It also defines how teams can collaborate around a stable behavioral contract.

---

## The Actions List as a Development Contract

In ADExMo, the Actions List is the official contract of the application behavior.

It defines:

- which domains exist
- which actions are available
- what each action does
- which inputs each action requires
- which outputs each action returns
- which business rules apply
- which constraints must be respected
- how each action can be invoked

This allows different teams to work on the same application without sharing internal implementation details.

The Actions List defines what the system can do.

It does not define how the system is implemented internally.

---

## Contract Instead of Internal Knowledge

Without a clear action contract, API and UI teams often depend on internal backend knowledge.

They need to know:

- which services exist
- which controllers are available
- which database tables are involved
- which internal methods must be called
- which validations are implemented where
- which side effects happen during execution

This creates unnecessary coupling.

With ADExMo, API and UI teams only need to know:

- the action name
- the action intent
- the required input
- the expected output
- the rules and constraints
- the possible errors or failure conditions

This reduces the dependency between implementation and integration.

---

## Benefits for API Teams

API teams can use the Actions List as the foundation for endpoint design.

Each endpoint can be mapped to one or more actions.

Example:

```text
POST /users -> user:create
POST /orders/{id}/process -> order:process
POST /invoices/{id}/generate -> invoice:generate
```

The API layer does not need to contain business logic.

Its responsibility is to:

1. receive the request
2. validate transport-level requirements
3. map request data to action input
4. invoke the action
5. return the action result

This keeps the API layer thin and predictable.

---

## Benefits for UI Teams

UI teams can use the Actions List as a functional reference for interface design.

For each action, the UI team can understand:

- which operation is available
- which form fields are required
- which optional parameters exist
- which outputs must be displayed
- which errors must be handled
- which user flows are supported

This allows UI development to begin without needing to inspect backend code.

The UI team can design screens, forms and workflows based on the action contract.

---

## Benefits for External Partners

ADExMo makes it easier to delegate API, UI or integration work to external companies.

An external team does not need to understand the internal architecture of the application.

It can work from the Actions List as a technical and functional reference.

This is especially useful when:

- the core backend is developed internally
- the UI is assigned to an external agency
- the API layer is developed by a separate team
- integrations are built by partners
- multiple vendors work on the same project

The Actions List becomes a shared contract between all parties.

---

## Backend Validation Before Integration

One of the strongest advantages of ADExMo is that backend logic can be completed and tested before UI or API integration.

Actions can be tested through:

- automated tests
- CLI execution
- internal runners
- integration test adapters

This means that the core business logic can be validated independently.

When the UI or API team starts integration, the backend behavior is already available and testable.

This reduces the common problem where frontend and backend are developed at the same time but neither side has a stable contract to work against.

---

## CLI as a Testing Interface

A CLI adapter can be used to execute actions directly.

Example:

```text
app user:create test@example.com --admin
app order:process 123
app invoice:generate 456
```

This gives developers and integrators a direct way to verify action behavior without using the UI.

The CLI does not replace the API or the UI.

It provides a practical execution interface for testing, debugging and validation.

---

## Reduced Meetings and Clarifications

A well-defined Actions List reduces repeated clarification work.

It answers questions such as:

- What can the system do?
- Which inputs are required?
- Which outputs are returned?
- Which business rules apply?
- Which operation should this button trigger?
- Which action should this endpoint call?
- Which errors must the UI handle?

It does not eliminate communication.

It makes communication more precise.

---

## Parallel Development

ADExMo supports parallel workstreams.

The core team can define and implement actions.

The API team can map endpoints to actions.

The UI team can design screens and flows from the Actions List.

The integration team can prepare external connections based on action contracts.

This reduces sequential dependency between teams.

Instead of waiting for a complete interface, teams can work from a shared behavioral model.

---

## Responsibility Separation

ADExMo creates clear responsibility boundaries.

### Core Team

The core team is responsible for:

- domains
- actions
- business rules
- validation
- execution behavior
- action tests

### API Team

The API team is responsible for:

- endpoint design
- request mapping
- response formatting
- authentication and transport concerns
- invoking actions correctly

### UI Team

The UI team is responsible for:

- user flows
- screens
- forms
- client-side interaction
- presenting outputs and errors
- invoking API endpoints linked to actions

### External Partners

External partners can work on API, UI or integrations by using the Actions List as the common reference.

They do not need direct access to internal backend implementation details unless explicitly required.

---

## Reduced Framework Dependency

The Actions List is independent from the framework used to implement the backend.

A UI or API team should not need to know whether the core logic is implemented with:

- Laravel
- Symfony
- Django
- FastAPI
- Express
- another backend framework

The contract is based on actions, not on framework internals.

This improves long-term maintainability and makes integrations less fragile.

---

## Better Onboarding

New developers can understand the system faster by starting from the Actions List.

Instead of reading controllers, routes, services and database tables first, they can begin with:

- domains
- actions
- business intent
- input contracts
- output contracts
- rules
- constraints

This gives them a clear map of what the application does before they study how it is implemented.

---

## Better Vendor Management

When working with external vendors, the Actions List can become part of the technical delivery package.

It can be used to define:

- what the vendor can rely on
- which operations are available
- which behavior is already implemented
- which UI or API functions must be built
- where responsibility boundaries are located

This reduces the risk of vendors making assumptions based on incomplete or informal explanations.

The contract is explicit.

---

## Practical Workflow

A practical ADExMo-based workflow can follow this sequence:

1. Define domains
2. Define actions inside each domain
3. Validate actions with the Action Definition Checklist
4. Implement the core actions
5. Test actions independently
6. Expose actions through CLI for validation
7. Map API endpoints to actions
8. Build UI flows based on the Actions List
9. Integrate external systems through adapters
10. Version the Actions List as the application evolves

This keeps the business logic stable while allowing interfaces to evolve independently.

---

## Example

### Action Definition

```markdown
### Action: user:create

Intent:
Create a new user account.

Input:
- email: string
- admin: bool = false

Output:
- created user
- validation error

Rules:
- email must be unique
- admin can only be assigned by authorized users

Constraints:
- email must be valid

Signature:
createUser(email: string, admin: bool = false)
```

### API Usage

```text
POST /users
```

The API team maps the request body to the action input.

### UI Usage

The UI team creates a user creation form with:

- email field
- optional admin toggle, if allowed
- validation error display
- success message

### CLI Usage

```text
app user:create test@example.com --admin
```

The same action is used by all interfaces.

---

## Risks and Constraints

Contract-based development only works if the Actions List is maintained carefully.

Common risks include:

- actions are not updated when behavior changes
- UI teams treat the Actions List as informal documentation
- API teams add business logic outside actions
- action definitions expose implementation details
- domains are poorly defined
- outputs and errors are not documented clearly

To avoid these problems:

- the Actions List must be versioned
- breaking changes must be explicit
- actions must be validated before implementation
- interfaces must not duplicate business rules
- the core team must own the action contract

---

## Key Principle

> The Actions List allows teams to build interfaces without knowing the internal implementation of the business logic.

---

## Summary

ADExMo turns business logic into a stable execution contract.

This allows:

- API teams to map endpoints to actions
- UI teams to design interfaces from action definitions
- backend teams to test logic before integration
- external partners to work without internal backend knowledge
- teams to work in parallel instead of waiting for each other

The result is a cleaner collaboration model built around executable business behavior.

One execution layer. Any interface.
