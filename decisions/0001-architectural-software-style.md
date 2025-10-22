# architectural software style

* Status: proposed
* Deciders: Jaime Ochoa de Alda Cerdan and Alejandro Garcia Prada
* Date: 2025-10-22

## Context and Problem Statement

The company needs a platform to manage purchasing processes, including customer management, orders, delivery routes, statistics, incidents, and payments via Stripe. The system must be scalable, secure, and capable of integrating with external services through HTTP.

## Decision Drivers

* RF-07 Comunicarse con servicio externo para datos de tráfico
* RF-10 Conectarse con la pasarela de pago

## Considered Options

* 0001-1-MCV
* 0001-2-Client-Server

## Decision Outcome

Chosen option: "0001-2-Client-Server", because it better suits the company’s requirements for distributed communication, HTTP-based integration with Stripe, and scalability for managing orders and logistics.

### Positive Consequences

* Compatible with cloud-based deployment and third-party integrations.

### Negative Consequences

* Possible latency in API calls.

## Pros and Cons of the Options

### 0001-1-MCV

The MVC model (Model–View–Controller) is a way to organize the system in three parts:
Model: Handles the data and business logic.

View: Displays the information to the user.

Controller: Connects the Model and View, managing user input and updating the View as needed.

More info: https://developer.mozilla.org/es/docs/Glossary/MVC

* Good, because it's very simple and has been used largely
* Good, because of clear responsability separation
* Bad, because of limited scalability
* Bad, because of the difficulties integrating asyncronous processes

### 0001-2-Client-Server

The client-server architecture is a distributed computing model in which tasks are divided between clients and servers. Clients are devices or applications that request services or resources, while servers are centralized systems that provide those resources, process data, and handle business logic. Communication between clients and servers typically occurs through standardized protocols such as HTTP or HTTPS.

* Good, because it ensures sensitive data never reaches the client and maintains a higher level of security
* Good, because HTTP/HTTPS communication makes it easy to connect with Stripe
* Bad, because The central server must handle all client requests, requiring proper load balancing and scaling resources
