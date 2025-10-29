# Specification of database relational or non-relational

* Status: proposed
* Deciders: Jaime Ochoa de Alda Cerdán
* Date: 2025-10-29

## Context and Problem Statement

After selecting a three-tier architecture it is necessary to specify the type of database required for the project, as both relational and non-relational databases bring their advantages and disadvantages.

## Decision Drivers

* RF-12 Notificaciones sobre el estado del pedido
* RF-14 Registro histórico de rutas y entregas

## Considered Options

* 0003-1-Relational Database
* 0003-2-Non-Relational Database

## Decision Outcome

Chosen option: 0003-1-Relational Database, because the project requires strong data consistency, referential integrity between entities (e.g., linking routes and incidents to specific orders), and transactional guarantees during the sequential processing phases of an order. Additionally, retrieving historical order data, delivery records, and statistics benefits from structured relationships and SQL queries.

### Positive Consequences

* Enables reliable tracking of all orders made by every client, preserving historical data.
* Supports ACID transactions, ensuring data integrity during multi-step operations (order processing, payment updates).
* Structured schema ensures consistency, validation, and controlled evolution of the domain model.


### Negative Consequences

* Reduced flexibility when adapting the data model; schema migrations may become complex in production environments.
* Vertical scalability may become a bottleneck under extreme load without additional strategies (partitioning, replication).
* May require performance optimization for frequently changing datasets (e.g., real-time statistics).

## Pros and Cons of the Options

### 0003-1-Relational Database

The Presentation layer communicates with the Business Logic layer through standard HTTP or HTTPS requA relational database organizes data into structured tables with predefined schemas and relationships between entities. It supports SQL as a standardized query language. This makes it ideal for applications that require strong consistency, complex queries, and referential integrity.ests. Each request includes the necessary context(headers, tokens, payloads), and the server responds with a structured message, typically in JSON format.

More info: https://en.wikipedia.org/wiki/Relational_database

* Good, because it enforces data integrity through primary keys, foreign keys, and constraints.
* Good, because SQL allows complex queries, joins, and aggregations needed for statistics and historical reports.
* Good, because it simplifies modeling of sequential order states and relationships with deliveries and incidents.
* Bad, because schema evolution requires coordinated migrations that may introduce downtime or complexity.
* Bad, because horizontal scalability is limited compared to some NoSQL solutions.
* Bad, because handling large volumes of unstructured telemetry data (e.g., live vehicle position streams) may require complementary storage strategies.

## 0003-2-Non-Relational Database

A non-relational (NoSQL) database stores data in flexible and schema-less structures (e.g., key-value, documents, column stores). It offers high scalability, flexible data models, and suitability for unstructured or rapidly evolving data.

More info: https://en.wikipedia.org/wiki/NoSQL

* Good, because it allows flexible schemas that can evolve without downtime.
* Good, because it scales horizontally with ease, supporting large amounts of real-time data.
* Good, because it can store event-based information (e.g., continuous tracking logs) efficiently.
* Bad, because eventual consistency may complicate multi-step workflows requiring strict state integrity.
* Bad, because complex relationships are harder to enforce, increasing application-level complexity.
* Bad, because analytics requiring joins become more inefficient.
