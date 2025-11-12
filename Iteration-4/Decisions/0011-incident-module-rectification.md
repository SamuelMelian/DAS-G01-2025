# Incident Module Rectification

* Status: proposed
* Deciders: Alejandro Garcia Prada
* Date: 2025-11-12

## Context and Problem Statement

In decision 0005, we chose a builder pattern to generate the incidents. We made a mistake because the incident entity is simple, with only basic attributes and minimal logic.

## Decision Drivers

* RF-09 Reportar incidencias

## Considered Options

* 0011-1-Single-class

## Decision Outcome

Chosen option: "0011-1-Single-class", because the incident module just have to made the proper incidents depending of the user type that is notifying it.This can be efficiently managed using a single class with appropriate methods, rather than introducing an unnecessary abstraction layer (e.g., a builder pattern) that offers no tangible benefits in this context.


### Positive Consequences

* Simpler and faster to implement
* Reduced number of classes
* Lower system coupling and fewer dependencies



### Negative Consequences

* Limited scalability if future incident creation becomes more complex


## Pros and Cons of the Options

## 0011-1-Single-class

With this Single Class solution, we consolidate all the logic for creating and managing incidents into a single, cohesive class. This class will be responsible for:

* Defining all the incident attributes (e.g., type, description,...).
* Providing factory methods or constructors to initialize incidents depending on the user type or context.

This approach emphasizes simplicity and maintainability, avoiding the unnecessary complexity of the builder pattern while still easy to extend if minor changes are needed in the future.

* Good, because of the straightforward implementation.
* Bad, because May require rework if complexity increases over time.







