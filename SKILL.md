---

name: adexmo-action-modeling
description: Use this skill to transform application analysis into ADExMo Domains, Actions, Action Definitions, and a validated Actions List. This skill models executable business behavior. It does not implement application code.
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

# ADExMo Action Modeling Skill

## Purpose

This skill helps transform application analysis into an ADExMo behavioral contract.

The main output is a validated Actions List.

ADExMo models application behavior as executable Actions, organized into Domains and independent from interfaces such as HTTP, CLI, UI, queues, controllers, or jobs.

This skill is used to:

* identify system responsibilities
* define Domains
* transform use cases into executable Actions
* write Action Definitions
* produce an Actions List
* validate that Actions are business-level, executable, and interface-independent

This skill must not be used to implement application code.

Implementation may be handled later by ABD, by a human development team, or by another controlled development process.

---

## Core Principle

ADExMo defines what the system can execute.

An Action is not an API endpoint, controller method, database operation, UI action, or internal helper.

An Action is a business or system capability that can be executed independently from any interface.

The Actions List is the contract of application behavior.

---

## When to Use This Skill

Use this skill when the user asks to:

* analyze an application for ADExMo
* define Domains
* create a Domain Map
* transform requirements into Actions
* transform use cases into Actions
* create an Actions List
* validate an existing Actions List
* write or review Action Definitions
* check whether candidate Actions are valid
* remove implementation leakage from Actions
* prepare an ADExMo contract for later implementation

---

## When Not to Use This Skill

Do not use this skill to:

* implement source code
* write controllers
* write API routes
* write database migrations
* design frontend screens
* create infrastructure scripts
* generate ORM models
* define deployment topology
* replace ABD implementation workflow
* make project architecture decisions unrelated to Actions

If the user asks for implementation before a valid Actions List exists, stop and request or produce the ADExMo Actions List first.

---

## Relationship with ABD

ADExMo and ABD have different responsibilities.

ADExMo owns Action modeling.

ABD owns controlled implementation based on decisions, Actions, rules, and Coding Agent instructions.

ADExMo may be used without ABD.

ABD may depend on ADExMo artifacts.

ADExMo must not depend on ABD.

The correct dependency direction is:

```text
ADExMo -> validated behavioral contract
ABD -> controlled implementation of that contract
```

If an ABD workflow requires an Actions List and the Actions List is missing, incomplete, or not ADExMo-compliant, the Coding Agent must stop ABD implementation and perform ADExMo modeling first.

---

## Expected Inputs

The user may provide any of the following:

* goals and scope
* business description
* existing documentation
* use cases
* actor list
* glossary
* business rules
* constraints
* existing API routes
* UI screens
* database schema
* legacy code
* rough notes
* stakeholder requirements

Not all inputs are mandatory.

When information is missing, make reasonable assumptions only if they are clearly marked as assumptions.

Do not invent business rules as facts.

---

## Recommended Workflow

Follow this workflow unless the user asks for a narrower task.

### 1. Establish Scope

Clarify:

* what the system does
* what the system does not do
* who uses it
* which responsibilities belong to the system
* which responsibilities belong to external actors or systems

Stop if the system boundary is too unclear to define Actions.

### 2. Stabilize Terminology

Identify important terms and potential ambiguities.

Prefer simple, stable domain language.

Do not use framework, database, API, or UI terminology as the basis for Actions.

### 3. Identify Actors

Identify human and system actors that trigger behavior.

Actors help determine where system responsibility begins.

### 4. Define Domains

Create Domains before defining Actions.

A Domain is a responsibility area of the system.

Valid Domains are based on business or system responsibility.

Invalid Domains include:

* api
* controller
* database
* repository
* service
* frontend
* backend
* model
* migration

Each Action must belong to exactly one Domain.

### 5. Identify Candidate Actions

Derive candidate Actions from:

* use cases
* actor goals
* business rules
* system responsibilities
* lifecycle transitions
* meaningful operations

Do not derive Actions directly from:

* API routes
* database tables
* UI buttons
* controllers
* services
* ORM models
* infrastructure components

Downstream artifacts may provide clues, but they must not drive the Actions List.

### 6. Write Action Definitions

For each Action, define:

* Domain
* Action name
* Intent
* Input
* Output
* Rules
* Constraints
* ADExMo Signature

The Action name must express business or system intent.

The Action Definition must not expose implementation details.

### 7. Validate Actions

Each Action must pass these checks:

* it represents a real business or system use case
* it has a single responsibility
* it is executable without an interface
* it has clear and minimal inputs
* it has defined outputs
* it has explicit rules and constraints
* it does not expose implementation details
* it belongs to exactly one Domain
* it uses stable terminology
* it has a predictable outcome

If an Action fails any check, revise it before accepting it.

### 8. Produce the Actions List

Create the Actions List as the official ADExMo behavioral contract.

The Actions List must be structured by Domains.

It should include links to individual Action Definition files when separate files are used.

### 9. Report Gaps

Before finalizing, report:

* missing Domains
* ambiguous Actions
* duplicated Actions
* Actions with implementation leakage
* Actions without clear inputs or outputs
* missing rules
* missing constraints
* out-of-scope behaviors
* assumptions that require human confirmation

---

## ADExMo Recommended Path Reference Map

The ADExMo Recommended Path defines the reference sequence used to move from early application analysis to a validated Actions List.

This Skill uses the Recommended Path as the reference model for producing and validating ADExMo artifacts.

ABD may use the same path to verify approval requirements for its own tasselli, but the path itself belongs to ADExMo.

The Recommended Path is not a heavy methodology.

It is a lightweight guidance sequence whose target artifact is always the validated ADExMo Actions List.

### Reference Table

| Order | Recommended Path Item        | Purpose                                                                                                      | Primary Reference                                                                                                                                |
| ----: | ---------------------------- | ------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------ |
|     1 | Goals & Scope Statement      | Defines the system perimeter and prevents out-of-scope Actions                                               | [`ADExMo-From-Analysis-to-Actions-List.md#1-goals--scope-statement`](ADExMo-From-Analysis-to-Actions-List.md#1-goals--scope-statement)           |
|     2 | Glossary                     | Stabilizes domain terminology used by Domains, Actions, inputs, outputs, and rules                           | [`ADExMo-From-Analysis-to-Actions-List.md#2-glossary`](ADExMo-From-Analysis-to-Actions-List.md#2-glossary)                                       |
|     3 | Actor Map                    | Identifies human and system actors that trigger system behavior                                              | [`ADExMo-From-Analysis-to-Actions-List.md#3-actor-map`](ADExMo-From-Analysis-to-Actions-List.md#3-actor-map)                                     |
|     4 | Context Diagram / System Map | Clarifies system boundaries and external responsibilities                                                    | [`ADExMo-From-Analysis-to-Actions-List.md#4-context-diagram--system-map`](ADExMo-From-Analysis-to-Actions-List.md#4-context-diagram--system-map) |
|     5 | Use Case List                | Lists supported operations from the actor perspective                                                        | [`ADExMo-From-Analysis-to-Actions-List.md#5-use-case-list`](ADExMo-From-Analysis-to-Actions-List.md#5-use-case-list)                             |
|     6 | Domain Map                   | Defines system responsibility areas used to structure the Actions List                                       | [`ADExMo-From-Analysis-to-Actions-List.md#6-domain-map`](ADExMo-From-Analysis-to-Actions-List.md#6-domain-map)                                   |
|     7 | Business Rules Summary       | Collects reusable business rules that affect Actions                                                         | [`ADExMo-From-Analysis-to-Actions-List.md#7-business-rules-summary`](ADExMo-From-Analysis-to-Actions-List.md#7-business-rules-summary)           |
|     8 | Use Case Detail              | Clarifies non-trivial behavior, alternatives, preconditions, postconditions, and exceptions                  | [`ADExMo-From-Analysis-to-Actions-List.md#8-use-case-detail`](ADExMo-From-Analysis-to-Actions-List.md#8-use-case-detail)                         |
|     9 | Sequence Diagrams            | Clarify complex interactions, external systems, async behavior, and Action boundaries                        | [`ADExMo-From-Analysis-to-Actions-List.md#9-sequence-diagrams`](ADExMo-From-Analysis-to-Actions-List.md#9-sequence-diagrams)                     |
|    10 | State Diagram                | Clarifies lifecycle constraints and valid state transitions                                                  | [`ADExMo-From-Analysis-to-Actions-List.md#10-state-diagram`](ADExMo-From-Analysis-to-Actions-List.md#10-state-diagram)                           |
|    11 | Mindmap                      | Supports exploration when the system is still unclear                                                        | [`ADExMo-From-Analysis-to-Actions-List.md#11-mindmap`](ADExMo-From-Analysis-to-Actions-List.md#11-mindmap)                                       |
|    12 | Constraint Summary           | Captures business, technical, legal, security, integration, and deployment constraints affecting Actions     | [`ADExMo-From-Analysis-to-Actions-List.md#12-constraint-summary`](ADExMo-From-Analysis-to-Actions-List.md#12-constraint-summary)                 |
|    13 | ADR When Needed              | Ratifies important decisions affecting Actions, Domains, interfaces, execution, or implementation boundaries | [`ADExMo-From-Analysis-to-Actions-List.md#13-adr-when-needed`](ADExMo-From-Analysis-to-Actions-List.md#13-adr-when-needed)                       |
|    14 | Draft Actions List           | Produces the first structured candidate Actions List                                                         | [`ADExMo-From-Analysis-to-Actions-List.md#14-draft-actions-list`](ADExMo-From-Analysis-to-Actions-List.md#14-draft-actions-list)                 |
|    15 | Validated Actions List       | Produces the final ADExMo behavioral contract                                                                | [`ADExMo-From-Analysis-to-Actions-List.md#15-validated-actions-list`](ADExMo-From-Analysis-to-Actions-List.md#15-validated-actions-list)         |

### Approval Requirement Usage

When this Skill is used to support ABD tasselli, each tassello must be checked against the relevant ADExMo Recommended Path items.

The Coding Agent must verify:

* whether the required ADExMo artifact exists
* whether the artifact satisfies its Stop Condition
* whether the artifact contributes to the Actions List
* whether open questions or assumptions remain
* whether the next ABD tassello can safely proceed

A tassello must not be considered approved only because a file exists.

A tassello may be considered ready only when the related ADExMo artifact satisfies its purpose, expected content, contribution to the Actions List, and Stop Condition.

### Mandatory vs Selective Path Items

The Recommended Path is progressive but not rigid.

The following items are usually mandatory before producing a serious Actions List:

| Item                         | Reason                                                    |
| ---------------------------- | --------------------------------------------------------- |
| Goals & Scope Statement      | Required to define system boundaries                      |
| Glossary                     | Required to stabilize terminology                         |
| Actor Map                    | Required to identify behavior initiators                  |
| Context Diagram / System Map | Required to separate system and external responsibilities |
| Use Case List                | Required to identify supported behavior                   |
| Domain Map                   | Required because Actions must be organized by Domain      |
| Business Rules Summary       | Required when rules affect Action behavior                |
| Constraint Summary           | Required when constraints affect Action behavior          |
| Draft Actions List           | Required before validation                                |
| Validated Actions List       | Required as the final ADExMo contract                     |

The following items are selective:

| Item              | Use When                                                                                          |
| ----------------- | ------------------------------------------------------------------------------------------------- |
| Use Case Detail   | Use for non-trivial or ambiguous use cases                                                        |
| Sequence Diagrams | Use for complex flows, async behavior, external systems, or unclear Action boundaries             |
| State Diagram     | Use when lifecycle states affect valid Actions                                                    |
| Mindmap           | Use for exploration before formalizing Domains or use cases                                       |
| ADR When Needed   | Use when a decision affects Actions, Domains, interfaces, execution, or implementation boundaries |

### Skill Rule

The Coding Agent must not skip directly from rough requirements to the Actions List when the system boundary, terminology, actors, Domains, rules, or constraints are unclear.

If the required path items are missing or incomplete, the Coding Agent must stop and produce or request the missing ADExMo artifact before validating the Actions List.

---

## Output Artifacts

Use the following recommended structure unless the project requires a different layout:

```text
docs/
  adexmo/
    domain-map.md
    actions-list.md
    business-rules-summary.md
    constraint-summary.md
    actions/
      <domain>/
        <action-name-kebab-case>.md
```

Example:

```text
docs/
  adexmo/
    actions-list.md
    domain-map.md
    actions/
      user/
        create-user.md
      order/
        process-order.md
      document-request/
        publish-document-request.md
```

---

## Naming Rules

Domain names must use lowercase-kebab-case.

Examples:

```text
user
order
document-request
access-control
inventory-movement
```

Action names must use lowerCamelCase.

Examples:

```text
createUser
processOrder
publishDocumentRequest
assignUserPermission
```

Action Definition file names must use lowercase-kebab-case.

Examples:

```text
create-user.md
process-order.md
publish-document-request.md
assign-user-permission.md
```

The file path is derived from Domain and Action name.

Do not add a required `File`, `Path`, or `Definition File` property inside Action Definitions.

---

## Action Definition Format

Use this structure for each Action Definition:

````markdown
# Action: <actionName>

## Domain

<domain-name>

## Intent

<short explanation of the result produced by the Action>

## Input

| Name | Type | Required | Description |
|---|---|---:|---|
| exampleId | int | yes | Example input |

## Output

<expected result or result type>

## Rules

- <business rule>
- <business rule>

## Constraints

- <constraint>
- <constraint>

## Signature

```text
actionName(inputName: type): OutputType
````

````

---

## Actions List Format

Use this structure for the Actions List:

```markdown
# ADExMo Actions List

## Version

0.1.0

## Purpose

This document defines the executable behavior contract of the application.

## Domains Overview

| Domain | Responsibility |
|---|---|
| user | Manages user identity and access |
| order | Manages order lifecycle |

## Actions Overview

| Domain | Action | Intent |
|---|---|---|
| user | createUser | Creates a new user account |
| order | processOrder | Processes an existing order |

## Actions by Domain

### user

#### createUser

Intent:
Creates a new user account.

Signature:
```text
createUser(email: string, admin: bool = false): UserResult
````

````

---

## Stop Conditions

Stop and ask for clarification when:

- the system boundary is unclear
- a candidate Action does not belong to any Domain
- a Domain is technical instead of responsibility-based
- an Action exposes implementation details
- an Action depends on HTTP, CLI, UI, controller, request object, or framework context
- the input contract is vague or uses generic containers such as `data`, `payload`, or `request`
- the output is undefined
- business rules are missing for non-trivial behavior
- two Actions appear to overlap
- a requested behavior is outside the declared scope
- the user asks for code implementation before the Actions List is valid

---

## Handling Existing Technical Artifacts

Existing API routes, database schemas, controllers, services, UI screens, and code may be used only as evidence.

They must not define the Actions directly.

When using technical artifacts, translate them back into business or system responsibilities.

Example:

```text
POST /api/orders/{id}/process
````

may suggest:

```text
processOrder(orderId: int)
```

But the Action must still be validated against business intent, rules, constraints, inputs, outputs, and Domain ownership.

---

## Validation Checklist

Before finalizing an Actions List, verify:

* every Action belongs to exactly one Domain
* every Domain represents a responsibility, not a technical layer
* every Action name uses lowerCamelCase
* every Action has clear intent
* every Action has explicit input
* every Action has explicit output
* every Action has rules and constraints when needed
* no Action exposes implementation details
* no Action depends on a specific interface
* no Action is just a CRUD operation unless it represents real system responsibility
* no downstream artifact redefines the Actions List
* assumptions are listed separately
* open questions are visible

---

## Final Response Style

When producing ADExMo artifacts:

* be concise
* use Markdown
* separate accepted content from assumptions
* mark open questions clearly
* prefer stable domain language
* avoid implementation details
* do not generate code unless explicitly asked after the Actions List is valid
* explain why invalid Actions are invalid
* propose corrected Action names when needed


