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

## Decision Outcome

Chosen option: 0007-1 – Strategy Pattern. It decouples algorithm selection from execution via a stable interface, enabling interchangeable implementations and clean evolution. It supports runtime selection driven by route context while keeping business logic independent of concrete algorithms. Compared to Template Method, it favors composition over inheritance, which limits coupling and makes extensions safer.

### Positive Consequences

* The system can evolve easily as new algorithms are introduced.
* Teams can work in parallel on different strategies without conflicts.
* Testing and debugging are simplified because each algorithm is isolated.


### Negative Consequences

* More types to manage (interface plus implementations) increases structural surface area.
* Requires a clear selection mechanism (e.g., a selector component) to keep choosing logic cohesive.

## Pros and Cons of the Options

### 0007-1–Strategy-Pattern

The Strategy Pattern encapsulates each optimization algorithm in its own class and allows them to be interchangeable at runtime. This makes it easy to add or replace algorithms without modifying existing code.

* Good, because algorithms are interchangeable and easy to extend.
* Good, because promotes SOLID principles and clean separation of concerns.
* Good, because supports runtime flexibility and dynamic selection.
* Good, because promotes decoupling and testability by isolating algorithm behavior.
* Bad, because More classes to manage; requires disciplined packaging and naming to avoid sprawl.
* Bad, because needs a well-defined selector to avoid scattering “which algorithm” logic across the codebase.

## 0007-2–Template-Method

The Template Method defines a skeleton of the optimization process, with common steps implemented in a base class and specific logic The Template Method defines a skeleton of the optimization process, with common steps implemented in a base class and specific logic in subclasses. It ensures consistency across algorithms but enforces a rigid inheritance structure in subclasses. It ensures consistency across algorithms but enforces a rigid inheritance structure.

* Good, because it standardizes shared steps across all algorithms, ensuring consistency in the workflow.
* Good, because it reduces code duplication by centralizing common logic in the base class.
* Good, because it provides a clear and predictable process structure, making the overall flow easier to understand.
* Bad, because it enforces inheritance, which reduces flexibility and can create rigid hierarchies.
* Bad, because modifying the base class to change the skeleton may impact all subclasses, introducing fragility.
* Bad, because it makes it harder to support algorithms that require different flows, leading to duplication or workarounds.