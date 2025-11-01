# Specification of database relational or non-relational

* Status: proposed
* Deciders: Jaime Ochoa de Alda Cerdán
* Date: 2025-11-01

## Context and Problem Statement

Within the presentation tier of the system architecture, it is necessary to identify the core domain classes that will be used by client-side applications (web, mobile, desktop) to interact with the business logic tier. These classes must represent the primary entities required by users to manage orders, deliveries, payments, and incidents, while supporting identity, access configuration, and platform-specific constraints.

Client applications need to display personal information, order status, delivery tracking, notifications, and reporting features in a consistent and structured way. Therefore, the identification of appropriate domain classes in the client module is required.

## Decision Drivers

* RF-02 Autenticar y autorizar a los usuarios
* RF-12 Notificaciones sobre el estado del pedido
* RF-09 Reportar incidencias
* RF-15 Almacenamiento de datos de compra.

## Considered Options

* 0006-1: User, ClientPlatform
* 0006-2: Customer, ClientProfile, PlatformPolicy
* 0006-3: Account, ClientContext

## Decision Outcome

Chosen option: 0004-1 (User, ClientPlatform), because these classes clearly separate personal user identity from platform-level policies and constraints, while maintaining a simple and intuitive domain structure. The User class represents personal data, account details, contact information, and references to orders and incidents. The ClientPlatform class encapsulates environment configuration, notification preferences, billing rules, and maximum request policies that apply to every user associated with it.

This separation improves maintainability, reduces coupling, and aligns with the system’s multi-platform and multi-user requirements.

### Positive Consequences

* User enables clear representation of identity, contact information, login credentials, and historical order tracking.
* ClientPlatform centralizes platform-wide settings (e.g., order limitations, notification channels) without duplicating data across users.
* Both classes integrate naturally with business domains such as deliveries, incidents, and payments.
* Improves scalability when adding additional client companies, as platform configurations do not need to be replicated per user.
* Reduces risk of data inconsistency by separating user attributes from operational constraints.


### Negative Consequences

* Additional complexity when associating multiple users to a single platform instance.
* ClientPlatform evolution may require schema adjustments in related classes later.
* Additional referential checks are needed to ensure that an order belongs to a valid user within a valid platform context.

## Pros and Cons of the Options

### 0006-1-User, ClientPlatform

TThis option models the client with a simple two-class structure:

- User acts as the main domain entity, storing core attributes such as identity, contact information, and authentication data relevant to order processing.

- ClientPlatform represents the platform through which the user interacts (e.g., web, mobile), encapsulating platform-dependent preferences, notification channels, or capability constraints.

This approach explicitly separates domain data from platform context while keeping the class model lightweight.

* Good, because it cleanly separates personal identity from platform policies.
* Good, because it aligns with the requirement of managing multiple client applications with consistent rules.
* Good, because it simplifies order and incident traceability per user.
* Bad, because it introduces an extra relational dependency.
* Bad, because modifying platform-level policies can affect all dependent users.

## 0006-2-Customer, ClientProfile, PlatformPolicy

This option provides a more decomposed representation:

- Customer is the root entity that identifies the client in the system.

- ClientProfile defines user preferences, personal configuration, and auxiliary attributes not required for identification but relevant to the user experience.

- PlatformPolicy captures rules, restrictions, or behavior dependent on the client’s interaction platform (e.g., allowed delivery windows, notification rules, UI constraints).

This model increases modularity and supports platform governance while keeping domain data organized.

* Good, because it provides finer granularity in configuration.
* Good, because responsibilities are more narrowly scoped.
* Bad, because it creates additional classes that increase cognitive load.
* Bad, because platform and customer configuration may fragment across files and modules.

## 0006-3-Account, ClientContext

This option models the client from a contextual and session-oriented perspective:

- Account represents authentication credentials, authorization level, and account-bound attributes such as status (active, suspended).

- ClientContext encapsulates runtime interaction data such as the active session, device metadata, locale, or environment parameters that may affect how operations are processed.

This structure favors architectures where the platform must adapt dynamically based on client state at execution time.

* Good, because it simplifies the number of classes.
* Good, because it reduces join operations.
* Bad, because mixing identity, preferences, and policy in one context violates single responsibility principles.
* Bad, because evolving requirements could lead to bloated data structures.