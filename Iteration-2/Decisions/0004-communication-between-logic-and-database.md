# Communication between logic and database

* Status: accepted
* Deciders: Alejandro Garcia Prada
* Date: 2025-10-30

## Context and Problem Statement

After selecting a three-tier architecture (client, business logic, and data), it is necessary to define how the business logic will communicate with the database layer. As we defined, the system will use a relational database, so the communication method must support SQL operations, transactional consistency, and structured data access.

## Decision Drivers

* RF-17 Almacenamiento y consulta de información persistente

## Considered Options

* 0004-1-TCP/IP

## Decision Outcome

Chosen option: "0004-1-TCP/IP Communication", because this communication between the business logic layer and the database provides a reliable, connection-oriented channel that ensures ordered data delivery, integrity, and compatibility across virtually all database systems. 

TCP/IP allows the business tier to establish persistent, secure sessions with the data tier—supporting transactional consistency, concurrent connections through pooling, and a possible encryption via TLS/SSL when needed.

### Positive Consequences

* Direct, efficient, and low-latency communication with the database.
* Standardized protocol compatible with relational database engines.
* Supports persistent connections and connection pooling to optimize performance.
* Ensures transactional integrity and reliable data delivery.

### Negative Consequences

* Increases dependency on database-specific configurations (drivers, ports, authentication).
* Debugging and traffic inspection are less straightforward compared to HTTP-based protocols.


## Pros and Cons of the Options

## 0004-1-TCP/IP

TCP/IP provides direct socket-level communication between the business logic and the database server. It is a universal standard supported by virtually all database systems and allows secure and reliable data transmission.

More info: https://www.rfwireless-world.com/terminology/tcp-ip-advantages-and-disadvantages


* Good, because it offers reliable, ordered, and error-checked delivery of data.
* Good, because it supports persistent connections, enabling efficient transaction handling.
* Good, because it can be secured via TLS/SSL for encrypted communication.
* Bad, because it increases dependency on database-specific configurations (drivers, ports).
* Bad, because it may require more effort for debugging and traffic inspection compared to HTTP-based protocols.


