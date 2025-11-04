# Route Optimization Algorithm Selection

* Status: proposed
* Deciders: Jaime Ochoa de Alda Cerdán
* Date: 2025-11-04

## Context and Problem Statement

The system must manage delivery routes and optimize them according to different conditions (RF-06, RF-14). There are several possible algorithms, such as shortest distance, fastest time with traffic, or capacity-based optimization. The architecture should allow the system to select the most appropriate algorithm depending on the context. The solution must also be extensible so that new algorithms can be added without disrupting existing functionality.

## Decision Drivers

* RF-06 Gestionar rutas / elegir algoritmo
* RF-14 Selección automática de algoritmo

## Considered Options

* 0007-1 – Strategy Pattern
* 0007-2 – Template Method
* 0007-3 – Hardcoded Conditional Logic

## Decision Outcome

Chosen option: 0007-1 – Strategy Pattern. This approach was selected because it encapsulates each algorithm in its own class, making them interchangeable and easy to extend. It directly supports runtime selection and aligns with SOLID principles, especially Open/Closed. Compared to the other options, it offers the best balance between flexibility, maintainability, and scalability.

### Positive Consequences

* The system can evolve easily as new algorithms are introduced.
* Teams can work in parallel on different strategies without conflicts.
* Testing and debugging are simplified because each algorithm is isolated.


### Negative Consequences

* The number of classes and abstractions grows, which may increase learning curve for new developers.
* Requires careful design of the selection logic to avoid complexity in the factory or context.

## Pros and Cons of the Options

### 0007-1–Strategy-Pattern

The Strategy Pattern encapsulates each optimization algorithm in its own class and allows them to be interchangeable at runtime. This makes it easy to add or replace algorithms without modifying existing code.

* Good, because algorithms are interchangeable and easy to extend.
* Good, because promotes SOLID principles and clean separation of concerns.
* Good, because supports runtime flexibility and dynamic selection.
* Good, because facilitates unit testing and mocking.
* Bad, because increases the number of classes and abstractions.
* Bad, because requires a factory or context object to manage selection.
* Bad, becuase developers must understand the pattern to avoid misuse.

## 0007-2–Template-Method

The Template Method defines a skeleton of the optimization process, with common steps implemented in a base class and specific logic The Template Method defines a skeleton of the optimization process, with common steps implemented in a base class and specific logic in subclasses. It ensures consistency across algorithms but enforces a rigid inheritance structure.in subclasses. It ensures consistency across algorithms but enforces a rigid inheritance structure.

* Good, because reduces code duplication by standardizing common steps.
* Good, because provides a clear workflow for all algorithms.
* Good, because easier for junior developers to follow a fixed template.
* Bad, because limits flexibility due to inheritance constraints.
* Bad, because adding new algorithms may require modifying the base class.
* Bad, because it can lead to fragile hierarchies if overused.

## 0007-3–Hardcoded-Conditional-Logic

This option implements all algorithms in a single class, using conditional statements to decide which one to apply. It is simple to implement initially but quickly becomes unmaintainable as the system grows.

* Good, because very simple to implement with minimal abstractions.
* Good, because easy to understand for small systems or prototypes.
* Bad, because violates Open/Closed Principle, requiring code changes for new algorithms.
* Bad, because becomes unmaintainable as the number of algorithms grows.
* Bad, because testing is harder due to tight coupling.
* Bad, because encourages duplication and scattered logic.