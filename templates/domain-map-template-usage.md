# Domain Map Template Usage

This document explains how to use `domain-map-template.md`.

Use this template to define the Domains that structure an ADExMo Actions List.

A Domain is a responsibility boundary of the system. It groups Actions that belong to the same area of behavior.

## When to Use This Template

Use `domain-map-template.md` before writing the final Actions List.

The Domain Map should be created when the team needs to:

- organize candidate Actions by responsibility
- avoid a flat and confusing Actions List
- clarify system boundaries
- prevent duplicated Actions
- separate business responsibilities from technical layers

The Domain Map is especially useful after the first use cases are known and before Action Definitions are finalized.

## Recommended Output File

The recommended output file is:

```text
docs/adexmo/domain-map.md
```

This file describes the Domain structure of the project.

Individual Action Definition files should then be placed under:

```text
docs/adexmo/actions/<domain>/<action-name-kebab-case>.md
```

Example:

```text
docs/adexmo/actions/document-request/publish-document-request.md
```

## Domain Naming Rules

Domain names must use `lowercase-kebab-case`.

Valid examples:

```text
user
order
invoice
document-request
access-control
inventory-movement
```

Invalid examples:

```text
User
DocumentRequest
documentRequest
document_request
api
controller
database
repository
frontend
service
```

A Domain name should be:

- short
- stable
- readable
- based on responsibility
- independent from technical implementation

Use compound Domain names only when a single word would be ambiguous.

Good compound names:

```text
document-request
access-control
payment-method
inventory-movement
```

Avoid names that are too long or too technical:

```text
user-permission-management
document-request-processing-system
api-access-control-layer
```

## How to Fill the Template

### Version

Use the version of the Domain Map.

Example:

```markdown
## Version

0.1.0
```

Use `0.x` versions while the Domain Map is still being drafted.

Use `1.0.0` or higher when it becomes a validated project artifact.

### Status

Use one of the following values:

```text
draft
validated
```

Use `draft` while the Domain Map is still being reviewed.

Use `validated` when the Domain Map is stable enough to support the Actions List.

### Scope

Describe the system boundary covered by the Domain Map.

Good example:

```markdown
This Domain Map covers the document request system used by an accounting firm to manage document requests, uploads, deadlines, and client responses.
```

Bad example:

```markdown
This Domain Map covers the Laravel backend.
```

The Scope must describe the system behavior, not the technical implementation.

### Domains Overview

Use the overview table to list all Domains and their responsibilities.

Example:

```markdown
| Domain | Responsibility |
|---|---|
| client | Manage client accounts and their relationship with the firm |
| request | Manage document and information requests sent to clients |
| document | Manage uploaded or attached documents |
| notification | Manage notification requests and delivery tracking |
```

The overview should be short and easy to scan.

### Domain Section

Create one detailed section for each Domain.

Example:

```markdown
## Domain: request
```

The Domain name in the section title must match the Domain name in the overview table.

### Responsibility

Describe what the Domain owns.

Good example:

```markdown
Manage document and information requests sent by the firm to clients.
```

Bad example:

```markdown
Contains request controllers, request services, and request database tables.
```

The responsibility must describe behavior, not implementation.

### Includes

List what belongs to the Domain.

Example:

```markdown
- create document requests
- assign requests to clients
- close requests
- reopen requests
- track request status
```

Use this section to clarify the positive boundary of the Domain.

### Excludes

List what does not belong to the Domain.

Example:

```markdown
- storing uploaded files
- authenticating users
- sending email messages directly
```

Use this section to prevent boundary confusion.

If the boundary is obvious, this section can still contain `none`.

### Related Actors

List actors that interact with this Domain.

Example:

```markdown
| Actor | Relationship with this Domain |
|---|---|
| Firm Operator | Creates and manages requests |
| Client User | Responds to assigned requests |
| Notification Service | Receives notification requests |
```

Actors help clarify where system responsibility begins.

### Related Use Cases

List use cases that belong to or affect the Domain.

Example:

```markdown
| Use Case | Description |
|---|---|
| Create document request | A firm operator creates a request for a client |
| Close document request | A firm operator marks a request as completed |
| Reopen document request | A firm operator reopens a completed or cancelled request |
```

Use cases are not Actions yet, but they help identify candidate Actions.

### Candidate Actions

List possible Actions that may later be defined in the Actions List.

Example:

```markdown
| Action | Intent |
|---|---|
| createDocumentRequest | Creates a document request assigned to a client |
| closeDocumentRequest | Marks a request as completed |
| reopenDocumentRequest | Reopens a completed or cancelled request |
```

Candidate Actions must use `lowerCamelCase`.

Do not include technical Actions.

Bad examples:

```text
saveRequestToDatabase
handleRequestPost
callNotificationApi
```

Good examples:

```text
createDocumentRequest
closeDocumentRequest
requestNotificationDelivery
```

### Rules

List rules that apply broadly to the Domain.

Example:

```markdown
- A request must always belong to one client.
- A closed request cannot receive new documents.
- Only firm operators can close requests.
```

Rules can later be attached to individual Action Definitions.

### Constraints

List constraints that affect the Domain.

Example:

```markdown
- Notification delivery is handled asynchronously.
- Uploaded documents are managed by the document Domain.
- The request Domain must not depend on HTTP request context.
```

Constraints should clarify boundaries, execution limits, permissions, or integration responsibilities.

### Notes

Use this section only when needed.

Good uses:

- open questions
- boundary clarification
- links to related ADRs
- pending review notes

Do not use Notes to hide unresolved Domain boundaries.

If the Domain is unclear, fix Responsibility, Includes, and Excludes instead.

## Minimal Example

```markdown
# ADExMo Domain Map

## Version

0.1.0

## Status

draft

## Scope

This Domain Map covers the document request system used by an accounting firm to manage requests, client responses, and uploaded documents.

## Domains Overview

| Domain | Responsibility |
|---|---|
| client | Manage client accounts and their relationship with the firm |
| request | Manage document and information requests sent to clients |
| document | Manage documents uploaded or attached to requests |
| notification | Manage notification requests and delivery tracking |

---

## Domain: request

## Responsibility

Manage document and information requests sent by the firm to clients.

## Includes

- create document requests
- assign requests to clients
- close requests
- reopen requests
- track request status

## Excludes

- storing uploaded files
- authenticating users
- sending email messages directly

## Related Actors

| Actor | Relationship with this Domain |
|---|---|
| Firm Operator | Creates and manages requests |
| Client User | Responds to assigned requests |
| Notification Service | Receives notification requests |

## Related Use Cases

| Use Case | Description |
|---|---|
| Create document request | A firm operator creates a request for a client |
| Close document request | A firm operator marks a request as completed |
| Reopen document request | A firm operator reopens a completed or cancelled request |

## Candidate Actions

| Action | Intent |
|---|---|
| createDocumentRequest | Creates a document request assigned to a client |
| closeDocumentRequest | Marks a request as completed |
| reopenDocumentRequest | Reopens a completed or cancelled request |

## Rules

- A request must always belong to one client.
- A closed request cannot receive new documents.
- Only firm operators can close requests.

## Constraints

- Notification delivery is handled asynchronously.
- Uploaded documents are managed by the document Domain.
- The request Domain must not depend on HTTP request context.

## Notes

Related ADRs:

- ADR-0003 - Use asynchronous notifications
```

## Validation Checklist

Before accepting a Domain Map, verify:

- [ ] Each Domain uses `lowercase-kebab-case`.
- [ ] Each Domain represents a responsibility, not a technical layer.
- [ ] Each Domain has a clear Responsibility section.
- [ ] Includes and Excludes clarify the Domain boundary.
- [ ] Candidate Actions use `lowerCamelCase`.
- [ ] Candidate Actions represent real use cases.
- [ ] Candidate Actions do not expose implementation details.
- [ ] No Domain is named after API, controller, database, repository, frontend, service, or another technical layer.
- [ ] Every relevant use case can be assigned to a Domain.
- [ ] No relevant behavior is duplicated across Domains.
- [ ] The Domain Map is stable enough to support the Actions List.

## Common Mistakes

### Using Technical Domains

Bad:

```text
api
controller
database
repository
frontend
service
```

Good:

```text
user
order
invoice
document-request
access-control
```

### Creating Domains That Are Too Broad

Bad:

```text
management
operations
system
data
```

Good:

```text
request
document
client
notification
```

### Creating Domains That Are Too Narrow

Bad:

```text
create-user
close-request
invoice-button
```

Good:

```text
user
request
invoice
```

A Domain groups Actions. It is not a single Action.

### Mixing Domain and Action Naming

Bad Domain:

```text
create-user
```

Good Domain:

```text
user
```

Good Action:

```text
createUser
```

### Duplicating Responsibilities

Bad:

```text
document-request
request-management
request-processing
```

Good:

```text
request
```

Use one clear Domain unless there is a real boundary difference.

### Letting the Database Drive the Domains

Bad:

```text
users-table
orders-table
invoice-records
```

Good:

```text
user
order
invoice
```

Domains are based on responsibilities, not tables.

## Stop Condition

Stop editing the Domain Map when:

- the system responsibilities are clearly grouped
- every relevant use case can be assigned to one Domain
- Domain names are stable
- Domain boundaries are clear
- technical layers are excluded
- candidate Actions can be drafted without confusion

Do not turn the Domain Map into a full architecture document.

The Domain Map exists to structure the Actions List.
