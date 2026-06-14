# ADExMo Templates

This directory contains reusable templates for applying ADExMo in real projects.

Templates are provided to help teams create clear, consistent, and lightweight ADExMo documentation.

They are not intended to turn ADExMo into a heavy methodology.

Their purpose is simple:

> help teams move from analysis to a clear, structured, and executable Actions List.

## Recommended, Not Mandatory

The templates in this directory are recommended starting points.

Projects adopting ADExMo may adapt their documentation structure when needed, especially when existing repository standards, documentation tools, or organizational constraints require a different layout.

However, adaptations should preserve the core ADExMo principles:

- business logic remains independent from interfaces
- application behavior is represented through executable Actions
- Actions are organized into Domains
- the Actions List remains the behavioral contract
- implementation details do not leak into Action names or definitions
- technical layers do not drive the model

Official ADExMo templates must remain consistent with the accepted ADExMo conventions.

## Template Files and Usage Files

Each reusable template should normally have two files:

```text
<name>-template.md
<name>-template-usage.md
```

### Template File

The `*-template.md` file contains the clean document structure.

It is meant to be copied and filled.

It should not contain long explanations, tutorials, or notes that users must delete before using it.

Example:

```text
action-definition-template.md
```

### Usage File

The `*-template-usage.md` file explains how to use the related template.

It may include:

- purpose of the template
- when to use it
- how to fill each section
- naming rules specific to that template
- examples
- validation checklists
- common mistakes
- stop conditions

Example:

```text
action-definition-template-usage.md
```

This separation keeps templates clean and usage guidance readable.

## Available Templates

| Template | Usage Guide | Purpose |
|---|---|---|
| [action-definition-template.md](action-definition-template.md) | [action-definition-template-usage.md](action-definition-template-usage.md) | Defines a single ADExMo Action |
| [actions-list-template.md](actions-list-template.md) | actions-list-template-usage.md | Defines the project Actions List as the behavioral contract |
| [domain-map-template.md](domain-map-template.md) | [domain-map-template-usage.md](domain-map-template-usage.md) | Defines the Domains that structure the Actions List |

## Suggested Project Output Files

When a project applies these templates, the generated files should usually be placed under:

```text
docs/adexmo/
```

Recommended structure:

```text
docs/
  adexmo/
    domain-map.md
    actions-list.md
    actions/
      <domain>/
        <action-name-kebab-case>.md
```

Example:

```text
docs/
  adexmo/
    domain-map.md
    actions-list.md
    actions/
      user/
        create-user.md
      document-request/
        publish-document-request.md
```

This structure is recommended because it keeps the Actions List central while allowing detailed Action Definitions to live in separate files.

## How to Use These Templates

A typical lightweight workflow is:

1. Create or update the Domain Map.
2. Identify candidate Actions for each Domain.
3. Create the Actions List.
4. Create separate Action Definition files when the Actions List becomes too large or when detailed Action documentation is needed.
5. Validate that each Action is executable, interface-independent, and free from implementation leakage.

The workflow is progressive, but not rigid.

Use only the templates that help clarify the Actions List.

Do not create documents just for the sake of completing a checklist.

## Current Core Template Set

The current core template set is intentionally small:

```text
templates/
  README.md
  action-definition-template.md
  action-definition-template-usage.md
  actions-list-template.md
  actions-list-template-usage.md
  domain-map-template.md
  domain-map-template-usage.md
```

This is enough to support the first practical use of ADExMo without overloading the repository.

Additional templates may be added later when they clearly support the path toward the Actions List.

## Future Candidate Templates

Possible future templates include:

```text
business-rules-summary-template.md
business-rules-summary-template-usage.md
use-case-list-template.md
use-case-list-template-usage.md
use-case-detail-template.md
use-case-detail-template-usage.md
constraint-summary-template.md
constraint-summary-template-usage.md
adr-template.md
adr-template-usage.md
```

These should be added only when they improve clarity, consistency, or practical adoption.

ADExMo should remain lightweight.

## What Templates Must Not Do

Templates must not:

- introduce framework-specific assumptions into the ADExMo core
- turn controllers, APIs, jobs, queues, or UI screens into sources of business logic
- make database structure drive Domains or Actions
- encourage generic Actions such as `handleData`, `processRequest`, or `manageEntity`
- introduce file path properties inside Action Definitions when the path can be derived
- replace architectural decisions that should be recorded in ADRs
- expand ADExMo into a generic software analysis methodology

Templates exist to clarify executable behavior.

They do not replace judgment.

## Contribution Notes

New templates or changes to existing templates should be proposed through repository issues before being treated as official ADExMo material.

A template contribution should explain:

- which problem the template solves
- how it helps reach or validate the Actions List
- why the existing templates are not enough
- whether it introduces any new convention
- whether an ADR is required

Small wording corrections may be proposed directly through a pull request.

Structural changes should be discussed first.

## Final Principle

Templates are useful only when they make the Actions List clearer.

If a template does not help define, organize, validate, or protect executable Actions, it does not belong in the ADExMo template set.
