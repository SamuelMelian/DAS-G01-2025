# Removal of Bridge in Client Tier

* Status: proposed
* Deciders: Jaime Ochoa de Alda Cerdán
* Date: 2025-11-12

## Context and Problem Statement

Previous iterations of the presentation tier relied on the Bridge Pattern, separating the abstraction (User, ClientPlatform) from its implementations (WebClient, DesktopClient, MobileClient). This structure aimed to support multiple client types with independent rendering logic.

However, with the architectural shift toward a centralized API Gateway, this bridge abstraction is no longer required. The gateway now acts as the unified access layer for all external clients—web, mobile, desktop, or future interfaces—handling routing, request transformation, and authentication uniformly.

Maintaining separate platform classes adds complexity without functional benefit, as all client types now interact with the same gateway endpoints.

The Bridge Pattern decision (0006-presentation-tier-description) is therefore annulled.

## Decision Drivers

* Simplify the presentation layer by removing unnecessary abstractions.
* Ensure support for new client types through the API Gateway, without requiring changes in the internal model.
* Centralize authentication and routing in a single access layer.
* Maintain compatibility with existing and future client origins (mobile, web, desktop).

## Considered Options

* 0013-1-Unified-Client-via-API-Gateway

## Decision Outcome

Chosen option: 0013-1-Unified-Client-via-API-Gateway.
The presentation tier is simplified by removing platform-specific client classes and keeping a single client model. All external clients (web, mobile, desktop) now interact with the system exclusively through the API Gateway, which handles routing and request normalization. 

Communication is performed via HTTP/HTTPS, making the internal design independent of the client’s origin. This avoids duplicating logic already handled by the gateway and keeps the system extensible for future client types.

### Positive Consequences

* Reduces architectural complexity, eliminating redundant layers no longer needed with a Gateway.
* Centralizes all client interactions, making the system easier to reason about and maintain.
* Improves extensibility, since new clients do not require new platform-specific classes, only Gateway configuration.
* Avoids duplicated responsibilities, since request formatting and adaptation are handled by the Gateway alone.

### Negative Consequences

* The presentation tier removes its platform-specific classes and responsibilities, which may require defining more detailed Gateway behavior in future iterations.
* Internal components lose platform-specific hooks, which may require alternative mechanisms if platform-dependent logic is ever needed.

## Pros and Cons of the Options

## 0013-1-Unified-Client-via-API-Gateway

Replace platform-specific classes with a single Client entity that interacts with the system exclusively through the API Gateway. The gateway manages all external requests, providing device-agnostic access.

* Good, because it centralizes all external communication through the Gateway.
* Good, because it removes unnecessary abstractions that no longer provide value.
* Good, because new client types can be added without modifying internal classes.
* Bad, because platform-specific rendering or behavior must now be handled entirely outside the system.







