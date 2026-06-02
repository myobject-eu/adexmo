# Contributing to ADExMo

Contributions are welcome if they preserve the core principles of ADExMo.

ADExMo is an architectural and documentation model based on executable actions. Contributions must improve clarity, consistency, practical adoption, or supporting documentation without weakening the model.

## Core Contribution Principles

Before proposing changes, contributors should verify that the proposal:

- keeps business logic independent from interfaces
- preserves the action-based execution model
- avoids framework-specific assumptions
- does not introduce implementation leakage into action definitions
- improves clarity, consistency, or practical adoption

These principles are mandatory. A contribution that conflicts with them should be redesigned before submission.

## What ADExMo Protects

ADExMo is not a framework-specific implementation and it is not a UI, API, CLI, or job system.

ADExMo defines how application behavior is represented and executed through actions. Interfaces only invoke actions. They must not become the source of business logic.

Contributions must therefore preserve the following boundaries:

| Area | Must remain true |
|---|---|
| Business logic | Defined as executable actions |
| Interfaces | Invoke actions, but do not own logic |
| Actions | Represent real use cases |
| Action definitions | Avoid implementation details |
| Domains | Group responsibilities, not technical layers |
| Actions List | Remains the behavioral contract |
| Frameworks | Remain optional adapters, not model foundations |

## Accepted Contribution Types

The following contributions are generally welcome:

- corrections to wording, grammar, structure, or examples
- improvements to documentation clarity
- better examples that explain ADExMo without adding framework dependency
- refinements to Action Definition guidance
- improvements to Domain grouping guidance
- additions that help teams reach a better Actions List
- ADR proposals for meaningful architectural decisions
- compatibility notes for specific frameworks, clearly marked as implementation guidance
- unofficial adapters, examples, or runtime notes, clearly separated from the core model

## Contributions That Are Usually Not Accepted

The following proposals are usually rejected:

- moving business logic into controllers, routes, commands, jobs, UI flows, or API endpoints
- defining actions around technical operations instead of business intent
- introducing database, ORM, controller, service, repository, or framework terms into action names
- making a specific framework part of the ADExMo core model
- treating the Actions List as optional documentation instead of a behavioral contract
- replacing actions with generic services, handlers, processors, or managers
- adding broad methodology content that does not help define or validate the Actions List
- weakening attribution, authorship, naming, or official governance rules

## Action Definition Rules

When contributing action examples or action-related documentation, make sure each action:

- belongs to exactly one Domain
- represents a real business or system use case
- has a clear verb plus object name
- has one responsibility only
- can be executed independently from HTTP, CLI, UI, queue, or another interface
- has explicit and minimal inputs
- has clear outputs
- includes relevant rules and constraints
- avoids implementation details in the action name, signature, and description

Valid examples:

```text
createUser(email: string, admin: bool = false)
processOrder(orderId: int, force: bool = false)
generateInvoice(orderId: int)
```

Invalid examples:

```text
handleRequest(requestObject)
saveUserToDatabase(data: mixed)
callApiAndPersistResult(payload)
validateAndStoreAndNotifyUser(userData)
```

## Domain Rules

Domains must represent areas of responsibility.

Valid Domains:

- user
- order
- invoice
- permission
- document
- request
- environment

Invalid Domains:

- controller
- api
- database
- repository
- frontend
- model
- service

A Domain is not a technical folder. It is a responsibility boundary for the Actions List.

## Documentation Contribution Rules

Documentation should be practical and lightweight.

A documentation contribution should answer at least one of these questions:

- Does it help identify Actions?
- Does it help organize Actions into Domains?
- Does it help validate Action boundaries?
- Does it clarify how business logic remains independent from interfaces?
- Does it help teams apply ADExMo without adding unnecessary methodology?

Documentation should not grow for its own sake.

## ADR Contribution Rules

ADR contributions are welcome when they record a meaningful architectural decision affecting ADExMo.

An ADR should be proposed when a decision affects:

- the definition of Actions
- the structure of the Actions List
- Domain organization
- execution model boundaries
- documentation path guidance
- relationship between ADExMo and implementations
- governance of official ADExMo materials

Do not create an ADR for small wording changes, simple typo fixes, or ordinary documentation cleanup.

## Framework-Specific Contributions

Framework-specific examples are allowed only when clearly presented as implementation guidance.

They must not redefine the ADExMo core model.

Acceptable:

- "Applying ADExMo in Laravel"
- "Example CLI Adapter"
- "Example HTTP Adapter"
- "Example Queue Adapter"

Not acceptable:

- making Laravel, Django, Symfony, FastAPI, Express, or any other framework part of the ADExMo core
- defining Actions according to framework routes or controllers
- changing the model to match a framework convention

## AI-Assisted Contributions

AI-assisted contributions are allowed.

Contributors may use Coding Agents, documentation assistants, RAG systems, code generators, or other automated tools to draft, review, or improve contributions.

The contributor remains responsible for the final content.

AI-assisted contributions must still:

- preserve attribution to ADExMo and myobject srls
- follow the ADExMo Community License
- respect the core principles of the model
- avoid rebranding or false claims of authorship
- clearly distinguish unofficial material from official ADExMo material

## Licensing of Contributions

By submitting a contribution, you confirm that:

- you have the right to submit it
- it does not knowingly infringe third-party rights
- it may be reviewed, edited, adapted, accepted, rejected, or incorporated by myobject srls
- accepted contributions may become part of the Official ADExMo Materials
- accepted contributions may be distributed under the ADExMo Community License or another license chosen by myobject srls for official ADExMo materials

Submission of a contribution does not automatically make it part of ADExMo.

A contribution becomes official only after explicit approval by myobject srls.

## Official Approval

Only myobject srls can approve changes to the official ADExMo model.

Approval may be given through:

- an official repository merge
- a written issue comment
- a pull request approval
- email confirmation
- a signed document
- another written channel controlled by myobject srls

Unofficial extensions, examples, translations, adapters, or guides must be clearly marked as unofficial unless approved.

## Recommended Contribution Process

1. Read the README and the LICENSE.
2. Check whether the proposal preserves the ADExMo core principles.
3. For small fixes, open a direct pull request.
4. For larger changes, open an issue first and explain the reason for the change.
5. For architectural changes, propose or reference an ADR.
6. Keep the proposal focused.
7. Explain how the change improves clarity, consistency, or practical adoption.
8. Wait for review and official approval before treating the change as part of ADExMo.

## Pull Request Checklist

Before submitting a pull request, verify:

- [ ] The change preserves business logic independence from interfaces.
- [ ] The change preserves the action-based execution model.
- [ ] The change avoids framework-specific assumptions in the core model.
- [ ] The change avoids implementation leakage into action definitions.
- [ ] The change improves clarity, consistency, or practical adoption.
- [ ] The change does not remove attribution to ADExMo or myobject srls.
- [ ] The change does not present unofficial material as official.
- [ ] The change is consistent with the ADExMo Community License.

## Review Criteria

A contribution may be accepted when it:

- strengthens the model
- improves documentation quality
- clarifies the Actions List
- improves adoption without adding unnecessary complexity
- preserves the separation between business logic and interfaces
- respects the official governance of ADExMo

A contribution may be rejected when it:

- weakens the model
- introduces ambiguity
- adds implementation leakage
- shifts responsibility to interfaces
- creates framework dependency
- conflicts with the license
- creates confusion about official ownership or approval

## Final Principle

ADExMo can evolve, but it must not lose its core idea:

> Business logic is defined as executable actions. Interfaces only invoke them.
