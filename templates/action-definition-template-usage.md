# Action Definition Template Usage

This document explains how to use `action-definition-template.md`.

Use this template to define a single ADExMo Action in a clear, executable, and interface-independent form.

For general naming rules, file naming rules, and recommended directory structure, see:

```text
templates/README.md
```

## When to Use This Template

Use `action-definition-template.md` when you need to document one specific Action.

An Action should represent a real business or system use case that can be executed independently from any interface.

Valid examples:

```text
createUser
processOrder
generateInvoice
publishDocumentRequest
```

Invalid examples:

```text
handleRequest
saveUserToDatabase
callApiAndPersistResult
validateAndStoreAndNotifyUser
```

## Recommended Output File

Action Definition files should follow this path convention:

```text
docs/adexmo/actions/<domain>/<action-name-kebab-case>.md
```

Examples:

```text
docs/adexmo/actions/user/create-user.md
docs/adexmo/actions/order/process-order.md
docs/adexmo/actions/document-request/publish-document-request.md
```

The file path is derived from the Domain and Action name.

Do not add a required `File`, `Path`, or `Definition File` property inside the Action Definition.

## How to Fill the Template

### Title

Use the Action name in `lowerCamelCase`.

Example:

```markdown
# ADExMo Action Definition: createUser
```

The title should match the value used in the `Name` section.

### Domain

Specify the Domain the Action belongs to.

The Domain must use `lowercase-kebab-case`.

Example:

```markdown
## Domain

user
```

A Domain must represent a system responsibility, not a technical layer.

Valid examples:

```text
user
order
invoice
document-request
access-control
```

Invalid examples:

```text
api
controller
database
repository
frontend
service
```

### Name

Specify the Action name in `lowerCamelCase`.

Example:

```markdown
## Name

createUser
```

The Action name should follow a verb plus object structure.

Good examples:

```text
createUser
processOrder
generateInvoice
assignUserPermission
```

Avoid vague or technical names.

### Intent

Write one sentence that explains the result of the Action.

Good example:

```markdown
Creates a new user account.
```

Avoid implementation details.

Bad example:

```markdown
Receives an HTTP request, validates the payload, saves the user to the database, and returns a JSON response.
```

The Intent must describe what the system achieves, not how it is implemented.

### Input

List the parameters required by the Action.

Each input should be explicit and minimal.

Example:

```markdown
| Name | Type | Required | Default | Description |
|---|---|---:|---|---|
| email | string | yes | none | Email address of the user to create |
| admin | bool | no | false | Whether the user should be created as an administrator |
```

Avoid generic containers such as:

```text
data
payload
requestObject
mixed
```

Use precise inputs instead.

### Output

List the result produced by the Action.

Example:

```markdown
| Name | Type | Description |
|---|---|---|
| userId | int | Identifier of the created user |
| status | string | Result status of the operation |
```

The Output should be clear enough for implementation, testing, API mapping, UI integration, or CLI execution.

### Rules

List the business rules applied by the Action.

Example:

```markdown
- The email address must be unique.
- The email address must be valid.
- Only authorized operators can create administrator users.
```

Rules should describe behavior that affects the Action result.

Do not include low-level implementation steps.

### Constraints

List limits, conditions, permissions, state restrictions, integration boundaries, or execution constraints.

Example:

```markdown
- Requires authenticated operator permissions.
- Does not send email directly.
- The Action must be executable without HTTP request context.
```

Constraints are especially useful when they preserve interface independence or define system boundaries.

### Signature

Write the executable ADExMo signature.

Example:

```text
createUser(email: string, admin: bool = false): CreateUserResult
```

The signature must use the same Action name defined in the `Name` section.

The signature should be stable because downstream interfaces and implementations depend on it.

### Notes

Use this section only when needed.

Good uses:

- open clarification points
- non-binding implementation notes
- links to related ADRs
- references to related Actions

Do not use Notes to hide missing rules, vague behavior, or unresolved Action boundaries.

If the Action is unclear, fix the main sections instead.

## Minimal Example

```markdown
# ADExMo Action Definition: createUser

## Domain

user

## Name

createUser

## Intent

Creates a new user account.

## Input

| Name | Type | Required | Default | Description |
|---|---|---:|---|---|
| email | string | yes | none | Email address of the user to create |
| admin | bool | no | false | Whether the user should be created as an administrator |

## Output

| Name | Type | Description |
|---|---|---|
| userId | int | Identifier of the created user |
| status | string | Result status of the operation |

## Rules

- The email address must be unique.
- The email address must be valid.
- Only authorized operators can create administrator users.

## Constraints

- Requires authenticated operator permissions.
- Must be executable without HTTP request context.
- Does not directly send notification emails.

## Signature

```text
createUser(email: string, admin: bool = false): CreateUserResult
```

## Notes

Related Actions:

- authenticateUser
- deactivateUser
```

## Validation Checklist

Before accepting an Action Definition, verify:

- [ ] The Action belongs to exactly one Domain.
- [ ] The Domain uses `lowercase-kebab-case`.
- [ ] The Action name uses `lowerCamelCase`.
- [ ] The Action name follows verb plus object structure.
- [ ] The Action represents a real business or system use case.
- [ ] The Action has one responsibility only.
- [ ] The Action can be executed independently from HTTP, CLI, UI, queue, or another interface.
- [ ] Inputs are explicit and minimal.
- [ ] Outputs are clear.
- [ ] Rules are defined.
- [ ] Constraints are defined where needed.
- [ ] The Signature matches the Action name and inputs.
- [ ] No implementation details leak into the Action name, Intent, Input, Output, Rules, Constraints, or Signature.
- [ ] No required file path property is included in the Action Definition.

## Common Mistakes

### Using a Technical Domain

Bad:

```text
api
controller
database
repository
```

Good:

```text
user
order
invoice
document-request
```

### Naming the Action After Implementation

Bad:

```text
saveUserToDatabase
callApiAndPersistResult
```

Good:

```text
createUser
synchronizeCustomer
```

### Depending on an Interface

Bad:

```text
handleUserPostRequest(requestObject)
```

Good:

```text
createUser(email: string, admin: bool = false)
```

### Using Generic Input

Bad:

```text
processOrder(data: mixed)
```

Good:

```text
processOrder(orderId: int, force: bool = false)
```

### Adding a File Path Property

Bad:

```markdown
File: docs/adexmo/actions/user/create-user.md
```

Good:

```markdown
## Domain

user

## Name

createUser
```

The file path is derived from Domain and Action name.

## Stop Condition

Stop editing the Action Definition when the Action is:

- meaningful
- assigned to one Domain
- named clearly
- free from implementation leakage
- executable without an interface
- clear in input and output
- governed by rules and constraints
- stable enough to be used by implementation teams
