# Specification of Database Removal

* Status: proposed
* Deciders: Jaime Ochoa de Alda Cerdán
* Date: 2025-11-12

## Context and Problem Statement

During the initial architectural design, several decisions were made regarding the use of a relational database, the communication layer between the logic and data tiers, and the encryption of that communication (0003, 0004, 0009).

However, after further analysis of the client’s requirements, it became clear that the system is not required to persist information. The application processes operational data in memory and communicates with external services, without storing internal datasets.

Therefore, the previous database-related decisions are no longer aligned with the actual business and functional requirements, and must be revoked.

## Decision Drivers

* The client explicitly stated that no persistent data storage is required.
* Security and compliance concerns related to stored data are irrelevant.

## Considered Options

* 0010-1-Remove-Database-Layer-Entirely
* 0010-2-Keep-lightweight-database-for-logging-or-caching

## Decision Outcome

Chosen option: 0010-1-Remove-Database-Layer-Entirely, because the client’s updated specifications confirm that no data persistence or storage of customer, order, or delivery information is required.

This decision supersedes and invalidates the following:

- 0003-Database Type (Relational or Non-relational)
- 0004-Communication Between Logic and Database
- 0009-Database Communication Encryption

The system will instead operate in a stateless mode, where transient data is processed in memory and exchanged directly between the application’s modules and external services. This simplifies the architecture and eliminates unnecessary database configuration, ORM logic, and encryption overhead.

### Positive Consequences

* Reduces system complexity and infrastructure costs.
* Eliminates the need for database management, migrations, or backups.
* Improves deployment simplicity and reduces integration points.
* Removes potential vulnerabilities related to data persistence or access control.

### Negative Consequences

* No long-term data persistence — historical information (orders, deliveries, incidents) cannot be recovered once processed.
* Some functionalities that relied on stored data (e.g., reporting, historical analytics) must be redesigned or dropped.
* Real-time dependencies increase — system availability now depends more strongly on external service uptime.

## Pros and Cons of the Options

### 0010-1-Remove-Database-Layer-Entirely

This option proposes completely removing the database component from the system architecture.
All data would be processed and exchanged in real time between the application’s modules and external APIs without any form of persistence. This approach simplifies the infrastructure and aligns strictly with the client’s updated specifications, which exclude the need for stored data.

* Good, because it aligns precisely with the client’s clarified requirements.
* Good, because it eliminates redundant architectural components.
* Bad, because it prevents future extensibility in case persistence becomes required.
* Bad, because removing the data layer makes it harder to simulate real scenarios for testing.

## 0010-2-Keep-Lightweight-Database-for-Logging-or-Caching

This option suggests retaining a minimal in-memory or lightweight database to temporarily store transient data, logs, or cache results that might assist during processing or debugging. Although it introduces a basic form of persistence, the goal is not to maintain business data but to improve runtime support and traceability during execution.

* Good, because it would allow minimal persistence of logs or transient session data.
* Good, because it could help debugging or auditing temporary events.
* Bad, because it contradicts the explicit client request of “no stored information.”
* Bad, because it reintroduces unnecessary components and costs for a non-required feature.