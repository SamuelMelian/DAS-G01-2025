# Specification of Client Module

* Status: proposed
* Deciders: Jaime Ochoa de Alda Cerdán
* Date: 2025-11-01

## Context and Problem Statement

Within the presentation tier of the system architecture, client-side applications (web, mobile, desktop) must interact with the business logic tier through domain classes that represent end users and the organizational platforms to which they belong. The system supports multiple users operating under the same corporate platform, sharing global policies, notification rules, and purchase preferences.

Client applications must display personal information, order history, delivery incidents, and billing preferences while enforcing platform-level constraints such as notification channels, allowed payment methods, and order limitations. To achieve this, it is necessary to identify the core domain classes that represent these responsibilities in a structured manner.

## Decision Drivers

* RF-01 CRUD de datos personales
* RF-03 CRUD de pedidos del cliente
* RF-09 Reportar incidencias
* RF-12 Notificaciones sobre el estado del pedido
* RF-15 Almacenamiento de datos de compra.

## Considered Options

* 0006-1: User, ClientPlatform 
    Two classes: one representing the authenticated user and another representing the corporate platform that defines shared policies for all associated users.
* 0006-2: Customer, ClientProfile, PlatformPolicy
    Three classes: one for basic identification, one for detailed preferences, and one for platform-wide rules.
* 0006-3: Account, ClientContext
    Two classes: one abstracting authentication data, and one grouping runtime context (locale, device, session metadata).

## Decision Outcome

Chosen option: 0006-1 (User, ClientPlatform), because it provides a clear separation between personal user attributes and shared platform policies without introducing unnecessary complexity. The User class models personal data, purchase history, and reported incidents, while ClientPlatform centralizes notification channels, payment preferences, and purchase rules applied to multiple users.

This separation introduces a maintainable multi-tenant structure, allowing operational policies to evolve without modifying individual users.

### Positive Consequences

* User class enables representation of personal data, historical orders, stored purchase preferences, and incident reporting.
* ClientPlatform encapsulates platform-level policies (e.g., allowed payment methods, notification channels) shared across multiple users, reducing data duplication.
* Simplifies enforcement of platform rules over a group of users.
* Supports scalability as new corporate clients can be added by instantiating new platform configurations.
* Enhances maintainability, as policy changes only occur at platform level.


### Negative Consequences

* Requires referential checks to ensure a user belongs to a valid platform.
* Additional complexity when assigning multiple users to the same platform instance.
* Changes to platform policies may unexpectedly affect all associated users.

## Pros and Cons of the Options

### 0006-1-User, ClientPlatform

This option models two distinct responsibilities:

- User: represents an authenticated end user interacting with the system, including personal details, order references, and incident submissions.

- ClientPlatform: represents the corporate platform under which users operate, defining notification channels, purchase rules, and allowed payment configurations.

* Good, because it cleanly separates personal identity from shared platform policies.
* Good, because it supports multi-tenant usage without duplicating policy data.
* Good, because orders and incidents can be traced directly to individual users.
* Bad, because relationships must be validated to prevent invalid platform associations.
* Bad, because platform-wide changes must be carefully managed to avoid unintended propagation.

## 0006-2-Customer, ClientProfile, PlatformPolicy

This option increases granularity by introducing three classes.

- Customer: identifies the client as the root domain entity.

- ClientProfile: stores personal preferences relevant to user experience.

- PlatformPolicy: enforces platform-wide behavioral rules.

* Good, because preferences and policies are isolated in separate classes.
* Good, because responsibilities are more narrowly scoped.
* Bad, because fragmentation increases cognitive load and maintenance complexity.
* Bad, because relationships between the three classes may become tightly coupled.

## 0006-3-Account, ClientContext

This option focuses on runtime context and authentication.

- Account: credentials, authorization, status (active/suspended).

- ClientContext: session-based metadata (device, locale).

This structure favors architectures where the platform must adapt dynamically based on client state at execution time.

* Good, because contextual data can be adapted at execution time.
* Good, because the class model is compact.
* Bad, because mixing identity, preferences, and runtime data violates single responsibility principles.
* Bad, because contextual attributes are not sufficient to satisfy long-term storage requirements (RF-15).