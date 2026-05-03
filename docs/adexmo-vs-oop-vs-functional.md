# ADExMo vs OOP vs Functional Programming

## Purpose

This document clarifies the relationship between ADExMo and common programming paradigms, especially:

- Object-Oriented Programming
- Functional Programming

ADExMo is not an alternative to these paradigms.

ADExMo defines how application behavior is exposed and executed.

OOP and Functional Programming define how the internal implementation may be structured.

---

## Core Definition

ADExMo is an execution interface for business logic.

It does not define how the system must be implemented internally.

It defines how the system exposes executable behavior through actions.

---

## What "Interface" Means in ADExMo

In ADExMo, the word "interface" does not mean:

- user interface
- HTTP API
- programming language interface
- framework contract

In ADExMo, interface means:

> the point through which the system exposes and executes its business behavior.

This interface is represented by actions.

---

## Actions as Execution Interface

An ADExMo action represents:

- a use case
- an executable unit
- a business operation
- a stable contract between the system and its consumers

Example:

```pseudo
order:process -> processOrder(orderId: int)
```

This definition does not describe how the order is processed internally.

It defines what the system is able to execute.

---

## What ADExMo Defines

ADExMo defines:

- what actions exist
- which domain each action belongs to
- which inputs each action requires
- which outputs each action produces
- which business rules apply
- how actions are invoked independently from interfaces

ADExMo focuses on execution.

---

## What ADExMo Does Not Define

ADExMo does not define:

- class structure
- inheritance hierarchy
- object lifecycle
- database schema
- internal algorithms
- framework-specific implementation
- whether the code is object-oriented or functional

These are implementation concerns.

---

## ADExMo and Object-Oriented Programming

Object-Oriented Programming defines how code is organized around objects.

Typical OOP concepts include:

- classes
- objects
- encapsulation
- inheritance
- polymorphism
- methods
- state

OOP is about internal structure.

ADExMo is about executable behavior.

---

## Difference Between ADExMo and OOP

| OOP | ADExMo |
|---|---|
| Defines code structure | Defines execution interface |
| Organizes behavior in objects | Organizes behavior in actions |
| Focuses on implementation model | Focuses on business execution contract |
| Uses classes and methods | Uses domains and actions |
| Describes how code is built | Describes what the system can execute |

---

## Using OOP to Implement ADExMo

ADExMo can be implemented using OOP.

Example:

```pseudo
class OrderActions {
    function processOrder(orderId: int) {
        ...
    }
}
```

In this case:

- the class is an implementation detail
- the method implements the action
- the ADExMo contract remains `order:process`
- external interfaces do not depend on the class structure

The action is the contract.

The class is one possible implementation.

---

## OOP Risk Without ADExMo

When a system is designed only through OOP structures, business behavior can become scattered across:

- controllers
- services
- models
- repositories
- event listeners
- jobs
- commands

This can make it difficult to understand where a use case actually begins and ends.

ADExMo reduces this ambiguity by forcing every relevant use case to be represented as a named executable action.

---

## ADExMo and Functional Programming

Functional Programming defines how code is organized around functions, composition and data transformation.

Typical Functional Programming concepts include:

- pure functions
- immutability
- function composition
- stateless behavior
- explicit input and output

Functional Programming is about implementation style.

ADExMo is about execution boundaries.

---

## Difference Between ADExMo and Functional Programming

| Functional Programming | ADExMo |
|---|---|
| Defines how functions are written | Defines which actions are executable |
| Focuses on purity and composition | Focuses on business execution contract |
| Avoids mutable state | Avoids interface-dependent business logic |
| Structures implementation around functions | Structures application behavior around actions |
| Describes how code behaves internally | Describes what the system exposes as executable behavior |

---

## Using Functional Programming to Implement ADExMo

ADExMo can be implemented using functional code.

Example:

```pseudo
function processOrder(orderId: int): Result {
    ...
}
```

In this case:

- the function implements the action
- the action defines the business contract
- the runtime or adapter invokes the function
- the interface does not own the business logic

Functional Programming can be a very natural implementation style for ADExMo because actions are explicit, input-driven and directly executable.

---

## ADExMo Above Implementation Paradigms

ADExMo sits above implementation paradigms.

```text
[ ADExMo Execution Interface ]
              |
              v
[ OOP / Functional / Procedural / Hybrid Implementation ]
              |
              v
[ Concrete Code ]
```

This means that different teams can implement actions differently while preserving the same action contract.

The internal implementation can change without changing the external execution model.

---

## Key Distinction

ADExMo answers this question:

> What can the system execute?

OOP and Functional Programming answer this question:

> How is the system implemented internally?

These are different architectural levels.

---

## Practical Example

### ADExMo Contract

```text
Domain:
order

Action:
order:process

Signature:
processOrder(orderId: int, force: bool = false)
```

### Possible OOP Implementation

```pseudo
class OrderProcessor {
    function process(orderId: int, force: bool = false) {
        ...
    }
}
```

### Possible Functional Implementation

```pseudo
function processOrder(orderId: int, force: bool = false) {
    ...
}
```

### Possible Interface Usage

```pseudo
POST /orders/{id}/process -> runAction("order:process", params)

app order:process 123 --force -> runAction("order:process", params)

queue job -> runAction("order:process", params)
```

The implementation may vary.

The action contract remains stable.

---

## Why This Matters

Without a clear execution interface, implementation details can leak into architecture.

This creates problems such as:

- controllers owning business rules
- APIs becoming the real behavior contract
- UI flows defining backend responsibilities
- duplicated logic across entry points
- inconsistent behavior between HTTP, CLI and jobs
- difficulty delegating API or UI development to external teams

ADExMo prevents this by making the action contract explicit.

---

## Implementation Freedom

ADExMo allows implementation freedom.

A team may use:

- OOP
- Functional Programming
- procedural code
- domain services
- application services
- framework-specific services
- hybrid approaches

The only requirement is that the action contract remains clear, executable and independent from the interface.

---

## Stable Contract, Flexible Implementation

Actions should be more stable than internal implementation.

Classes may be refactored.

Functions may be reorganized.

Services may be split.

Repositories may change.

Frameworks may evolve.

But the action contract should remain stable unless the behavior of the system changes.

This makes ADExMo useful as a bridge between:

- analysis
- backend implementation
- API development
- UI development
- testing
- external integrations

---

## Common Misunderstandings

### "ADExMo replaces OOP"

No.

ADExMo does not replace OOP.

OOP can be used to implement ADExMo actions.

---

### "ADExMo replaces Functional Programming"

No.

Functional Programming can be used to implement ADExMo actions.

---

### "ADExMo is just a service layer"

No.

A traditional service layer is often internal and not directly executable as a formal contract.

ADExMo makes the executable business behavior explicit, named, structured and reusable across interfaces.

---

### "ADExMo is an API design pattern"

No.

APIs are only one possible adapter.

ADExMo exists before the API layer.

An API may expose actions, but it does not define them.

---

### "ADExMo is a CLI model"

No.

FastCLI can be used as a CLI adapter for ADExMo.

But ADExMo is broader than CLI.

The CLI is only one way to invoke actions.

---

## Design Rule

Do not start by asking:

> Which controller, class or endpoint do we need?

Start by asking:

> Which action does the system need to execute?

After the action is clear, the implementation model can be chosen.

---

## Summary

ADExMo is not a programming paradigm.

It is an execution model.

It defines the operational interface of the business logic through actions.

OOP and Functional Programming remain valid implementation models.

The relationship is simple:

```text
ADExMo defines what to execute.
OOP and Functional Programming define how to implement it.
```

---

## Final Principle

> Actions define what the system does.
>
> Implementation paradigms define how the system does it.
