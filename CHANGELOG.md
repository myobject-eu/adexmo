# Changelog

All notable changes to ADExMo will be documented in this file.

ADExMo follows semantic versioning for public documentation, templates, and model evolution.

Version format:

```text
MAJOR.MINOR.PATCH[-pre-release]
```

Pre-release versions such as `alpha`, `beta`, or `rc` indicate that the model is usable but still subject to refinement before a stable release.

---

## [1.0.0-alpha] - 2026-06-14

### Added

- Initial public ADExMo model definition as Action-Driven Execution Model.
- Core principle: business logic is represented as executable Actions.
- Separation between business logic and interfaces.
- Definition of Actions as the primary execution units of application behavior.
- Definition of the ADExMo Actions List as the contract of application behavior.
- Definition of Domains as the structural layer of the Actions List.
- Initial documentation path guidance from analysis artifacts to the validated Actions List.
- Initial positioning of ADExMo as an execution interface, independent from OOP, Functional Programming, frameworks, APIs, CLI, UI, or queue systems.
- Initial relationship between ADExMo and FastCLI, with FastCLI positioned as a possible CLI adapter.
- Initial relationship between ADExMo and ABD, with ABD positioned as an operational application of ADExMo for controlled Coding Agent based development.
- Initial ADExMo Community License 1.0.
- Initial `CONTRIBUTING.md`.
- Initial template directory structure.
- Initial `templates/README.md`.
- Initial `templates/action-definition-template.md`.
- Initial `templates/action-definition-template-usage.md`.
- Initial `templates/actions-list-template.md`.
- Initial `templates/domain-map-template.md`.
- Initial `templates/domain-map-template-usage.md`.

### Defined

- Action names use `lowerCamelCase`.
- Action Definition file names use `lowercase-kebab-case.md`.
- Domain names use `lowercase-kebab-case`.
- Domain directories use the same value as the Domain name.
- Action Definition file paths are derived from Domain and Action name.
- Action Definitions do not require a `File`, `Path`, or `Definition File` property.
- `actions-list.md` may link to individual Action Definition files for navigation.
- Template files and usage files are separated:
  - `*-template.md`
  - `*-template-usage.md`

### Documentation

- Added ADR-000: FastCLI - Attribute-Driven Declarative CLI.
- Added ADR-001: PHPFastCLI - PHP Implementation of FastCLI.
- Added ADR-002: PythonFastCLI - Python Implementation of FastCLI.
- Added ADR-003: ADExMo - Action-Driven Execution Model.
- Added ADR-004: ADExMo Actions List - Contract of Application Behavior.
- Added ADR-005: ADExMo Domains - Structural Layer of the Actions List.
- Added ADR-006: ADExMo Documentation Path Guidance.
- Added ADR-007: ADExMo Template and File Naming Conventions.
- Added ADR-009: ABD as a Natural Application of ADExMo.
- Added ADExMo introductory and positioning documentation.
- Added ADExMo action definition checklist.
- Added ADExMo documentation path from analysis to Actions List.
- Added Laravel minimal setup example.

### Governance

- Defined contribution expectations.
- Defined GitHub Issues as the preferred channel for proposing contributions.
- Defined that official changes to ADExMo require review and approval by myobject srls.
- Defined that ADExMo may be used freely with attribution.
- Defined that modifications, extensions, and official variants require approval before being presented as official ADExMo material.

### Known Gaps

- `templates/actions-list-template-usage.md` is still pending.
- Additional templates may be added later only if they support the path toward the Actions List without making ADExMo a heavy general-purpose methodology.
- GitHub Issue and Pull Request templates are still pending.
- Stable `1.0.0` should be released only after the initial template set, repository structure, README references, and contribution workflow are reviewed as complete.
