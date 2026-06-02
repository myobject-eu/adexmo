# From Analysis to Actions List

## Purpose

This document defines a lightweight documentation path for moving from basic application analysis to an ADExMo Actions List.

It is not a complete software analysis methodology.

It does not teach how to write use cases, mindmaps, UML diagrams, wireframes, API specifications, or database schemas.

Its purpose is narrower:
> provide analysts, software architects, and technical teams with a practical path for producing the minimum useful documentation required to define clear, executable, and verifiable ADExMo Actions.

In ADExMo, documentation is not produced for its own sake.

Documentation is useful when it helps define, organize, validate, or protect the Actions List.

## ADExMo Positioning

ADExMo defines application behavior as a set of executable Actions.

The Actions List is the central contract of that behavior.

The documentation path described here helps transform early analysis into that contract.

```
Basic application documentation
  goals, actors, boundaries, use cases, domains, rules, constraints
        |
        v
ADExMo interpretation
  identification of executable business behavior
        |
        v
Actions List
  versioned contract of application behavior
        |
        v
Implementation and integration
  core logic, tests, API, UI, CLI, jobs, external systems
```

ADExMo does not replace software analysis.

ADExMo gives software analysis a concrete target:
> a clear, structured, versioned, and executable Actions List.

## Core Rule

If an artifact does not help define, organize, validate, or protect the Actions List, it does not belong to the ADExMo documentation path.

## What This Document Does Not Cover

This document does not define:

- database schema design
- API design
- UI design
- frontend routes
- controller structure
- ORM models
- deployment topology
- infrastructure scripts
- detailed UML notation
- general requirements engineering

Those artifacts may be useful, but they are downstream from the Actions List.

They consume or implement the Actions List.

They should not drive it.

---

# Recommended Path

## Overview

| Order | Artifact                     | Role                                   | Usage       |
| ----- | ---------------------------- | -------------------------------------- | ----------- |
| 1     | Goals & Scope Statement      | Defines system perimeter               | Required    |
| 2     | Glossary                     | Stabilizes terminology                 | Required    |
| 3     | Actor Map                    | Identifies initiators of behavior      | Required    |
| 4     | Context Diagram / System Map | Defines external boundaries            | Required    |
| 5     | Use Case List                | Lists supported operations             | Required    |
| 6     | Domain Map                   | Structures the Actions List            | Required    |
| 7     | Business Rules Summary       | Collects reusable rules                | Recommended |
| 8     | Use Case Detail              | Clarifies non-trivial behavior         | Selective   |
| 9     | Sequence Diagrams            | Clarify complex interactions           | Selective   |
| 10    | State Diagram                | Clarifies lifecycle constraints        | Selective   |
| 11    | Mindmap                      | Supports exploration                   | Optional    |
| 12    | Constraint Summary           | Captures constraints affecting Actions | Required    |
| 13    | ADR When Needed              | Ratifies important decisions           | Continuous  |
| 14    | Analysis Completeness Gate   | Validates readiness before Actions     | Required    |
| 15    | Draft Actions List           | First Action contract draft            | Required    |
| 16    | Validated Actions List       | Final behavioral contract              | Required    |

The path is progressive, but not rigid.

Required artifacts should exist for every project.

Selective artifacts should be produced only when they clarify the Actions List.

Optional artifacts are working tools and should not become authoritative unless explicitly promoted.

**Note on the Glossary:** for systems with an unfamiliar domain, the Glossary may be produced in parallel with artifacts 3 through 5 and consolidated before writing the Domain Map. The important constraint is that terminology is stable before Domains and Actions are named.

**Note on ADRs:** ADRs are not a late-stage activity. They can and should be produced at any point during the analysis when a decision with meaningful alternatives and consequences is made. The position of artifact 13 in this table indicates that ADRs are a continuous tool, not a one-time step before the Draft Actions List.

---

# 1. Goals & Scope Statement

## Purpose

Define what the system does, what it does not do, and for whom it exists.

This document sets the first boundary of the application.

## ADExMo Context

The Goals & Scope Statement defines the outer perimeter of the future Actions List.

A candidate Action should not be accepted if it falls outside the declared system scope.

## Before Writing

The team should know:

- the general business problem
- the target users or organizations
- the expected value of the application
- the main exclusions or limits of the system

## Expected Output

- system goal
- target users
- included responsibilities
- excluded responsibilities
- major assumptions
- main external dependencies

## Action List Contribution

This artifact helps decide whether a candidate Action belongs to the system.

## Example Representation

```
# Goals & Scope Statement

## Goal

The system manages document requests between an accounting firm and its clients.

## In Scope

- create document requests
- assign requests to clients
- upload requested documents
- track request status
- notify users about pending deadlines

## Out of Scope

- accounting calculation
- invoice generation
- tax filing submission
- payment processing

## Target Users

- accounting firm operators
- accounting firm administrators
- client users
```

## Stop Condition

Stop when the system boundary is clear enough to reject out-of-scope Actions.

---

# 2. Glossary

## Purpose

Clarify the meaning of key domain terms.

The glossary prevents ambiguity before it enters the Actions List.

## ADExMo Context

Action names, Domain names, inputs, outputs, and rules must use stable terminology.

If terminology is unstable, Actions become unstable.

## Before Writing

The team should have:

- an initial understanding of the business domain
- recurring terms used by stakeholders
- known words that may have different meanings for different people

## Expected Output

- domain terms
- short definitions
- ambiguity notes
- rejected or deprecated terms when useful

## Action List Contribution

This artifact stabilizes:

- Action names
- Domain names
- input names
- output names
- business rules

## Example Representation

```
# Glossary

| Term    | Definition                                        | Notes                          |
|---------|---------------------------------------------------|--------------------------------|
| Client  | Organization served by the accounting firm        | Not the same as user           |
| User    | Person with access to the system                  | Can belong to a firm or client |
| Request | A document or information request sent to a client| Can have deadlines and status  |
| Practice| Operational work unit managed by the firm         | May contain multiple requests  |
```

## Stop Condition

Stop when the terms required to understand the first Actions are clear.

The glossary can evolve later.

---

# 3. Actor Map

## Purpose

Identify who interacts with the system and what responsibility each actor has.

## ADExMo Context

Actors help identify where system responsibility begins.

This is important because an ADExMo Action should start where the system takes responsibility for executing behavior.

## Before Writing

The team should know:

- who uses the system
- which external systems interact with it
- which roles have different responsibilities

## Expected Output

- human actors
- system actors
- short responsibility description
- main interaction type

## Action List Contribution

This artifact helps derive use cases and identify candidate Actions triggered by actors.

## Example Representation

```
# Actor Map

| Actor                | Type   | Responsibility                                         |
|----------------------|--------|--------------------------------------------------------|
| Firm Admin           | Human  | Configures users, clients, and permissions             |
| Firm Operator        | Human  | Creates and manages document requests                  |
| Client User          | Human  | Uploads documents and responds to requests             |
| Notification Service | System | Sends email or system notifications                    |
```

## Stop Condition

Stop when all primary initiators of system behavior are identified.

---

# 4. Context Diagram / System Map

## Purpose

Show the system as a black box and clarify its external boundaries.

This is not an internal architecture diagram.

## ADExMo Context

The Context Diagram helps separate:

- what belongs to the system
- what belongs to external actors
- what belongs to external systems

This prevents Actions from including responsibilities that belong somewhere else.

## Before Writing

The team should have:

- Goals & Scope Statement
- Actor Map
- known external systems or services

## Expected Output

- system boundary
- external actors
- external systems
- main interactions

## Action List Contribution

This artifact helps avoid Actions that cross system boundaries incorrectly.

## Example Representation

```
[Client User]
     |
     | uploads documents
     v
[Document Request System]
     |
     | sends notifications
     v
[Notification Service]

[Firm Operator]
     |
     | creates requests
     v
[Document Request System]
```

## Stop Condition

Stop when it is clear what the system owns and what external actors or systems own.

---

# 5. Use Case List

## Purpose

List the main operations the system must support from the actor perspective.

This is still macroanalysis.

Use cases are not Actions yet.

## ADExMo Context

The Use Case List is one of the main sources for candidate Actions.

However:

- not every use case becomes one Action
- some use cases may produce multiple Actions
- some use cases may stay outside the Actions List if they describe navigation, presentation, or user-to-user interaction

## Before Writing

The team should have:

- Goals & Scope Statement
- Actor Map
- Context Diagram / System Map

## Expected Output

- use case identifier
- actor
- use case name
- short expected result

## Action List Contribution

This artifact provides the first structured list of behaviors that may later become Actions.

## Example Representation

```
# Use Case List

| ID     | Actor         | Use Case                   | Expected Result                          |
|--------|---------------|----------------------------|------------------------------------------|
| UC-001 | Firm Admin    | Create client account      | A new client account is available        |
| UC-002 | Firm Operator | Create document request    | A request is assigned to a client        |
| UC-003 | Client User   | Upload requested document  | The document is available to the firm    |
| UC-004 | Firm Operator | Close document request     | The request is marked as completed       |
```

## Stop Condition

Stop when the main business operations are visible and grouped by actor.

Do not detail every edge case here.

---

# 6. Domain Map

## Purpose

Define the main areas of responsibility of the system.

Domains organize the Actions List.

## ADExMo Context

In ADExMo, Domains are a structural layer of the Actions List.

A Domain must represent a business or system responsibility, not a technical layer.

Valid Domains describe responsibilities such as:

- user management
- documents
- requests
- billing
- permissions
- environments
- bookings
- reporting

Invalid Domains are technical containers such as:

- controller
- API
- database
- repository
- service
- frontend

## Before Writing

The team should have:

- Goals & Scope Statement
- Glossary
- Use Case List
- enough domain understanding to group responsibilities

## Expected Output

- Domain name
- responsibility description
- included behaviors
- excluded behaviors when useful

## Action List Contribution

Every Action must belong to exactly one Domain.

The Domain Map becomes the structural foundation of the Actions List.

## Example Representation

```
# Domain Map

## client

Responsibility:
Manage client accounts and their relationship with the firm.

Includes:
- create client account
- update client data
- deactivate client account

Excludes:
- user authentication
- document upload

## request

Responsibility:
Manage document and information requests sent to clients.

Includes:
- create request
- assign request
- close request
- reopen request

Excludes:
- physical document storage
- notification delivery

## document

Responsibility:
Manage documents uploaded or attached to requests.

Includes:
- upload document
- validate document
- remove document

Excludes:
- request lifecycle
- client account management
```

## Stop Condition

Stop when every relevant use case can be assigned to a stable Domain.

If a use case does not fit any Domain, either the Domain Map is incomplete or the use case is out of scope.

---

# 7. Business Rules Summary

## Purpose

Collect business rules that influence system behavior.

This document avoids scattering important rules across use cases and Actions.

## ADExMo Context

Rules are a core part of every Action Definition.

A Business Rules Summary helps keep rules consistent before they are attached to specific Actions.

## Before Writing

The team should have:

- Use Case List
- Domain Map
- known policy, operational, or business constraints

## Expected Output

- rule identifier
- rule statement
- affected Domain
- affected use cases or candidate Actions when known

## Action List Contribution

Rules from this document are later referenced or embedded into Action Definitions.

## Example Representation

```
# Business Rules Summary

| ID     | Rule                                                               | Domain  | Applies To                          |
|--------|--------------------------------------------------------------------|---------|-------------------------------------|
| BR-001 | A client account must have a unique tax identifier                 | client  | createClientAccount                 |
| BR-002 | A closed request cannot receive new documents                      | request | closeRequest, uploadDocument        |
| BR-003 | Only firm operators can create document requests                   | request | createDocumentRequest               |
| BR-004 | A client user can only see requests assigned to its client account | request | listClientRequests                  |
```

## Stop Condition

Stop when the rules needed to define predictable Actions are visible.

Do not document every implementation check here.

---

# 8. Use Case Detail

## Purpose

Detail only the use cases that contain meaningful logic, ambiguity, alternatives, or important rules.

Do not detail every use case by default.

## ADExMo Context

Use Case Detail helps identify:

- Action boundaries
- preconditions
- alternative flows
- postconditions
- errors
- business rules
- required inputs and outputs

## Before Writing

The team should have:

- Use Case List
- Domain Map
- Business Rules Summary

## Expected Output

- use case goal
- actor
- preconditions
- main flow
- alternative flows
- postconditions
- exceptions
- rules
- candidate Actions when identifiable

## Action List Contribution

This artifact helps transform broad use cases into precise candidate Actions.

## Example Representation

```
# UC-002 - Create Document Request

## Actor

Firm Operator

## Goal

Create a document request and assign it to a client.

## Preconditions

- the operator is authenticated
- the client account exists
- the requested document type is valid

## Main Flow

1. The operator selects a client.
2. The operator defines the requested document type.
3. The operator sets a deadline.
4. The system creates the request.
5. The system makes the request visible to the client.

## Alternative Flows

- If the client is inactive, the request is rejected.
- If the deadline is missing, the system applies the default deadline policy.

## Postconditions

- the request exists
- the request is assigned to the client
- the request has an initial open status

## Candidate Actions

- createDocumentRequest
```

## Stop Condition

Stop when the use case is clear enough to define one or more candidate Actions.

Do not turn this into implementation documentation.

---

# 9. Sequence Diagrams

## Purpose

Clarify interaction order only for flows involving multiple Domains, external systems, asynchronous processes, or complex responsibility boundaries.

Sequence diagrams are selective.

They are not required for every use case.

## ADExMo Context

Sequence diagrams help determine whether a behavior should be:

- one Action
- multiple Actions
- an Action plus an external integration
- an Action plus an asynchronous job

## Before Writing

The team should have:

- Use Case Detail for the relevant flow
- Domain Map
- known external systems or asynchronous steps

## Expected Output

- involved actors or systems
- interaction order
- responsibility boundaries
- relevant system calls or events at conceptual level

## Action List Contribution

This artifact helps avoid oversized Actions and clarify execution boundaries.

## Example Representation

```
Firm Operator -> System: create document request
System -> Request Domain: create request
Request Domain -> Client Domain: verify client is active
Request Domain -> Notification Service: request notification delivery
System -> Firm Operator: return created request
```

## Stop Condition

Stop when the Action boundary and external responsibilities are clear.

Do not model internal implementation calls unless they affect the Action contract.

---

# 10. State Diagram

## Purpose

Clarify lifecycle rules for entities with important states.

Use this only when state transitions affect valid Actions.

## ADExMo Context

State diagrams help define which Actions are allowed or forbidden depending on current state.

## Before Writing

The team should have:

- Domain Map
- Business Rules Summary
- use cases involving lifecycle transitions

## Expected Output

- entity or concept with states
- allowed states
- valid transitions
- forbidden transitions when relevant

## Action List Contribution

This artifact helps define constraints and rules inside Action Definitions.

## Example Representation

```
Request states:

draft -> open -> completed
open -> cancelled
completed -> reopened
cancelled -> reopened

Forbidden:
completed -> draft
cancelled -> completed
```

Candidate Actions:

```
createDocumentRequest
publishDocumentRequest
closeDocumentRequest
cancelDocumentRequest
reopenDocumentRequest
```

## Stop Condition

Stop when allowed and forbidden transitions are clear enough to define Action constraints.

---

# 11. Mindmap

## Purpose

Explore domain areas when the system is still unclear.

Mindmaps are working tools.

They are not mandatory deliverables.

## ADExMo Context

A mindmap may help discover missing Domains, use cases, actors, and rules before the Actions List is drafted.

It should not become the source of truth.

## Before Writing

The team should have:

- a rough understanding of the system
- unresolved domain ambiguity
- need for exploratory grouping

## Expected Output

- possible Domains
- possible actors
- possible operations
- open questions
- rough relationships

## Action List Contribution

This artifact supports exploration before formalizing the Domain Map and Use Case List.

## Example Representation

```
Document Request System
  clients
    create client
    deactivate client
    assign users
  requests
    create request
    assign request
    close request
    reopen request
  documents
    upload document
    validate document
    reject document
  notifications
    send reminder
    notify completion
```

## Stop Condition

Stop when the team can move from exploration to structured artifacts.

Do not version mindmaps as authoritative documentation unless there is a specific reason.

---

# 12. Constraint Summary

## Purpose

Define the constraints that affect the Actions List or its implementation boundaries.

This document should be brief.

## ADExMo Context

Constraints may influence:

- Action signatures
- rules
- validation
- execution mode
- integration limits
- interface independence

However, technical constraints should not leak into Action names.

## Before Writing

The team should have:

- system scope
- Domain Map
- known business, technical, legal, security, or deployment constraints

## Expected Output

- business constraints
- security constraints
- technical constraints
- integration constraints
- deployment constraints
- data constraints

## Action List Contribution

This artifact helps define Action rules, input limits, output expectations, and execution requirements.

## Example Representation

```
# Constraint Summary

## Business Constraints

- A client user can only access requests belonging to its client account.
- A request must always belong to one client.

## Security Constraints

- All Actions require an authenticated user unless explicitly public.
- Role checks must be applied before executing protected Actions.

## Technical Constraints

- The application must support asynchronous notification delivery.
- Uploaded documents must be stored outside the database.

## Integration Constraints

- Notification delivery is handled by an external service.
```

## Stop Condition

Stop when constraints that affect Action behavior are documented.

Do not use this document as a full technical architecture specification.

---

# 13. ADR When Needed

## Purpose

Record important decisions that affect the Actions List, execution model, architecture, boundaries, or implementation strategy.

ADR documents should not be created for every small preference.

## ADExMo Context

ADR documents are useful when a decision changes how Actions are defined, grouped, executed, validated, exposed, or versioned.

**ADRs are a continuous tool.** They can be produced at any point during the analysis when a decision with meaningful alternatives and consequences needs to be ratified. The position of this artifact in the table does not mean ADRs are written only at the end of the analysis. A decision made during the Domain Map phase or after a Sequence Diagram is resolved should be recorded immediately in an ADR, not deferred.

## Before Writing

The team should have:

- a real decision to ratify
- meaningful alternatives
- consequences worth documenting
- impact on Actions, Domains, interfaces, or implementation

## Expected Output

- context
- problem
- decision
- alternatives considered
- consequences

## Action List Contribution

ADR documents protect the Actions List from unstable or implicit architectural assumptions.

## Example Representation

```
# ADR-0003 - Use asynchronous notifications

## Context

Some Actions trigger notifications to users.

## Problem

Notification delivery may fail or be delayed.

## Decision

Actions that require notifications will not send them directly.
They will create a notification request handled asynchronously.

## Consequences

- createDocumentRequest returns after the request is created
- notification delivery status is tracked separately
- failed notification delivery does not invalidate request creation
```

## Stop Condition

Stop when the decision is clear, ratified, and usable by the team.

---

# 14. Analysis Completeness Gate

## Purpose

Verify that the analysis is complete and stable before starting the Draft Actions List.

This is a mandatory checkpoint, not a suggestion.

## ADExMo Context

Moving from analysis to the Actions List is not automatic.

Without a deliberate completeness check, the Draft Actions List absorbs unresolved ambiguities, missing rules, unstable Domains, and implicit constraints. These problems are significantly more expensive to fix once Actions are defined.

The gate protects the quality of the Actions List by ensuring that every upstream artifact required to write stable Action contracts is present and ratified.

## Before Passing the Gate

The team must verify:

- Goals & Scope Statement is ratified
- Glossary covers the terms needed to name Actions and Domains
- Actor Map identifies all primary initiators of behavior
- Use Case List covers the main operations to be supported
- Domain Map is stable and every use case belongs to a Domain
- Business Rules Summary captures the rules needed to define Action behavior
- Constraint Summary is ratified
- All selective artifacts required by the specific project have been produced
- All open ADRs are ratified or explicitly deferred with documented intent
- No use case or Domain remains unresolved or ambiguous

## Gate Outcome

If all conditions are satisfied, the team proceeds to the Draft Actions List.

If one or more conditions are not satisfied, the team identifies the missing artifacts and produces them before proceeding.

A team may choose to proceed with known gaps only if that choice is explicitly ratified and documented, stating what is missing and why the decision to proceed is acceptable.

## Action List Contribution

This gate ensures that the Draft Actions List is built on a complete and stable analytical foundation.

---

# 15. Draft Actions List

## Purpose

Create the first structured list of candidate Actions.

This is where the previous documentation begins to converge.

## ADExMo Context

The Draft Actions List is not final.

It is used to validate whether the analysis can be expressed as executable business behavior.

## Before Writing

The team should have passed the Analysis Completeness Gate and have:

- Goals & Scope Statement
- Glossary
- Actor Map
- Use Case List
- Domain Map
- relevant Business Rules
- relevant constraints

## Expected Output

For each candidate Action:

- Domain
- Action name
- intent
- input
- output
- rules
- constraints
- signature

## Action List Contribution

This is the first version of the Actions List.

It is reviewed, corrected, split, merged, renamed, or rejected before validation.

## Example Representation

```
# Draft Actions List

## Domain: request

### Action: createDocumentRequest

Intent:
Creates a document request and assigns it to a client.

Input:
- clientId: int
- documentTypeId: int
- deadline: date|null

Output:
- requestId: int
- status: string

Rules:
- the client account must exist
- the client account must be active
- the document type must be valid
- if deadline is missing, the default deadline policy applies

Constraints:
- only firm operators can create requests
- the request starts in open status

Signature:
createDocumentRequest(clientId: int, documentTypeId: int, deadline: date|null = null): CreateDocumentRequestResult
```

## Stop Condition

Stop when every relevant use case is represented by one or more candidate Actions, and every candidate Action belongs to a Domain.

---

# 16. Validated Actions List

## Purpose

Produce the final Actions List used as the contract of application behavior.

## ADExMo Context

The Validated Actions List is the official ADExMo output.

It defines what the system can execute, independently from API, UI, CLI, jobs, or integrations.

## Before Writing

Before finalizing this document, the team should have:

- reviewed Draft Actions List
- validated Domain ownership
- removed technical Actions
- removed duplicates
- clarified inputs and outputs
- attached rules and constraints
- checked Action independence from interfaces

## Expected Output

For each validated Action:

- Domain
- Action name
- intent
- input contract
- output contract
- rules
- constraints
- ADExMo signature

## Action List Contribution

This is the Actions List.

It becomes the reference for:

- implementation
- testing
- API mapping
- UI integration
- CLI adapter
- jobs
- external integrations

## Example Representation

```
# ADExMo Actions List

## Domain: request

### Action: createDocumentRequest

Intent:
Creates a document request assigned to a client.

Input:
- clientId: int
- documentTypeId: int
- deadline: date|null = null

Output:
- requestId: int
- status: RequestStatus

Rules:
- clientId must reference an active client account
- documentTypeId must reference an enabled document type
- if deadline is null, the system applies the default deadline policy

Constraints:
- requires firm operator permissions
- creates the request in open status
- does not directly deliver notifications

Signature:
createDocumentRequest(clientId: int, documentTypeId: int, deadline: date|null = null): CreateDocumentRequestResult
```

## Stop Condition

Stop when each Action is:

- meaningful
- assigned to one Domain
- free from implementation leakage
- executable without an interface
- clear in input and output
- governed by rules and constraints
- stable enough to be used by implementation teams

---

# Downstream Artifacts

The following artifacts are downstream from the Actions List:

- database schema
- API specification
- UI wireframes
- frontend routes
- controller design
- ORM models
- infrastructure scripts
- deployment topology

These artifacts may consume the Actions List, but they must not redefine it.

## Principle

The Actions List defines what the system executes.

Downstream artifacts define how that behavior is stored, exposed, displayed, deployed, or integrated.

---

# Relationship with Implementation

After the Actions List is validated, implementation can proceed through any suitable programming paradigm or framework.

ADExMo does not prescribe how the system is internally implemented.

It defines the execution contract.

The implementation must preserve the Actions List as the source of behavioral truth.

Interfaces should invoke Actions.

They should not duplicate business logic.

---

# Summary

ADExMo uses documentation to reach a concrete target:
> the Actions List as the executable contract of application behavior.

This path keeps analysis focused, prevents premature technical design, and gives implementation teams a stable behavioral foundation.
