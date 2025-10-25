# Communication between tiers

* Status: proposed
* Deciders:Alejandro Garcia Prada
* Date: 2025-10-25

## Context and Problem Statement

After selecting a three-tier architecture (client, business logic, and data), it is necessary to define how the layers will communicate with each other. The system must support integration with external services while ensuring interoperability, security, and scalability.

The challenge is to choose a communication protocol and data format that:

* Enables integration with third-party APIs (e.g., Stripe, traffic data).
* Maintains layer independence and modularity.


## Decision Drivers

* RF-01 CRUD de datos personales
* RF-04 Actualizar estado del pedido
* RF-07 Comunicarse con servicio externo para datos de tráfico
* RF-10 Conectarse con la pasarela de pago

## Considered Options

* 0002-1-HTTP/S-Communication
* 0002-gRPC

## Decision Outcome

Chosen option:  "0002-1-HTTP/S Communication", because this communication between the client, business logic, and data layers allows the server to maintain necessary state information during ongoing operations.

This approach combines the universality and compatibility of HTTP with the flexibility to preserve context for multi-step business processes (e.g., order lifecycle, payment authorization, delivery updates).

### Positive Consequences

* Preserves the ability to manage complex, stateful workflows.
* Compatible with third-party HTTP services and internal APIs.
* Easier debugging, monitoring, and testing with standard HTTP tools.


### Negative Consequences

* Increased complexity in state management and session synchronization when scaling horizontally.
* Slightly higher latency due to multiple HTTP exchanges between tiers.


## Pros and Cons of the Options

### 0002-1-HTTP/S Communication

Communication based on HTTP/HTTPS between layers of the n-tier architecture. The client sends requests to the business logic server, which may in turn interact with the data layer or external services. Data is typically transmitted in JSON or XML format.

More info: https://developer.mozilla.org/en-US/docs/Web/HTTP

* Good, because it is universally supported by web, mobile, and third-party clients.
* Good, because it allows integration with third-party APIs, such as Stripe or traffic data services.
* Good, because it enables the business logic layer to maintain state for multi-step workflows (e.g., order lifecycle, payment processing, delivery updates).
* Good, because debugging and monitoring are easier using standard HTTP tools (Postman, curl, browser developer tools).
* Good, because integration with existing firewalls, proxies, and load balancers is straightforward.
* Bad, because it has higher network overhead per request-response compared to binary protocols.
* Bad, because multiple HTTP exchanges between layers can introduce additional latency.

## 0002-2-gRPC

gRPC is a high-performance Remote Procedure Call (RPC) framework that uses HTTP/2 and Protobuf for binary message serialization. It allows clients and servers to communicate efficiently, supports bidirectional streaming, and enforces strict contracts via .proto files.

More info: https://grpc.io/

* Good, because binary messages are more compact than JSON/XML, improving performance.
* Good, because bidirectional streaming allows real-time notifications or continuous data flows.
* Good, because it natively supports multiple languages, making it ideal for heterogeneous service stacks.
* Bad, because not all browsers support native gRPC; web clients require gRPC-Web.
* Bad, because infrastructure is more complex (persistent connections, load balancers, firewalls).
* Bad, because debugging and monitoring are more difficult compared to HTTP/REST.
