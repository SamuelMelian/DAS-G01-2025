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

* Requires additional implementation units to be maintained, increasing the amount of logic the team must track across the codebase.
* Requires a clear selection mechanism (e.g., a selector component) to keep choosing logic cohesive.

## Pros and Cons of the Options

### 0007-1–Strategy-Pattern

The Strategy Pattern encapsulates each optimization algorithm in its own class and allows them to be interchangeable at runtime. This makes it easy to add or replace algorithms without modifying existing code.

* Good, because algorithms are interchangeable and easy to extend.
* Good, because it isolates the varying route-optimization behavior behind a common interface, avoiding conditional logic scattered across the codebase.
* Good, because supports runtime flexibility and dynamic selection.
* Good, because promotes decoupling and testability by isolating algorithm behavior.
* Bad, because more classes to manage; requires disciplined packaging and naming to avoid sprawl.
* Bad, because needs a well-defined selector to avoid scattering “which algorithm” logic across the codebase.

## 0007-2–Template-Method

The Template Method defines a skeleton of the optimization process, with common steps implemented in a base class and specific logic The Template Method defines a skeleton of the optimization process, with common steps implemented in a base class and specific logic in subclasses. It ensures consistency across algorithms but enforces a rigid inheritance structure in subclasses. It ensures consistency across algorithms but enforces a rigid inheritance structure.

* Good, because shared steps in route processing remain consistent across algorithms.
* Good, because placing common behavior in the base class reduces duplicated logic.
* Good, because it provides an explicit sequence of operations, making the optimization procedure predictable.
* Bad, because enforcing inheritance restricts flexibility and increases coupling to a single hierarchy.
* Bad, because changes in the base workflow risk affecting all subclasses simultaneously, creating fragility.
* Bad, because algorithms with different execution flows are harder to support, leading to duplication or workaround logic.