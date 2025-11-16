# External Traffic Service

* Status: proposed
* Deciders: Alejandro Garcia Prada
* Date: 2025-11-16

## Context and Problem Statement

One of the pillars of our system is ensuring that distribution is adequate. For that, optimized route management through the two algorithms is necessary. But for them to be effective, we need estimated traffic data. Therefore, we need to connect to an external service to obtain traffic estimates.

## Decision Drivers

RF-07: Comunicación con servicio externo de tráfico

## Considered Options

* 0013-1-Google Maps API
* 0013-1-HERE Traffic API

## Decision Outcome

Chosen option: "0013-1-HERE Traffic API", because it provides the most suitable functional capabilities for our routing algorithms and distribution workflows. HERE offers real-time traffic data, historical traffic patterns, road-segment-level speed information, and incident reports, all of which directly improve route optimization and ETA (Estimated Time of Arrival) accuracy. Its APIs are designed for logistics and fleet routing, making it functionally the best fit for our use case.

### Positive Consequences

* Provides the functional data required for both optimization algorithms (real-time traffic, predicted traffic, road speeds).
* Supports detailed road-segment traffic information, enabling more accurate cost matrices.
* Includes incident and congestion information, improving route reliability.
* Offers modular APIs (Traffic, Routing, Incidents) aligned with fleet-management requirements.


### Negative Consequences

* Requires greater familiarity with HERE’s set of APIs and data layers.
* Some highly granular data may require combining multiple endpoints.

## Pros and Cons of the Options

## 0013-1-Google Maps API

Google Maps  API provides traffic-aware routing, estimated arrival times, congestion modeling, and alternative routes using Google’s extensive real-time traffic data. It is easy to integrate, widely known, and highly reliable. Functionally, it excels in urban accuracy, but it provides less granular road-segment traffic detail compared to HERE.
More info: https://serpapi.com/google-maps-api

* Good, because real-time traffic and ETA calculations are highly accurate, especially in dense urban areas.
* Good, because integration is very simple and documentation is extensive.
* Good, because it automatically handles routing variations and optimizes ETA predictions.
* Good, because it offers a unified API for routing, distance estimation, and traffic-aware outputs.
* Bad, because it provides less detailed road-segment traffic data for low-level optimization.
* Bad, because traffic prediction (historical patterns) is limited compared to HERE.

## 0013-1-HERE Traffic API

HERE Traffic API provides real-time traffic levels, predicted speeds based on historical patterns, congestion levels, road incidents, and segment-level speed data. Functionally, it supports advanced routing scenarios typical in fleet logistics, making it ideal for feeding our optimization algorithms.

More info: https://www.here.com/platform/routing
* Good, because it provides both real-time and predictive traffic information needed for algorithmic routing.
* Good, because it includes road-segment-level speeds, enabling higher-precision cost calculations.
* Good, because incident, closure, and congestion data allow more reliable routing decisions.
* Good, because its API suite is designed for logistics and fleet operations, matching our system needs.
* Bad, because integration requires understanding multiple HERE services (Traffic Flow, Traffic Incidents, Routing).



