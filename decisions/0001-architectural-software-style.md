---
# These are optional metadata elements. Feel free to remove any of them.
status: "proposed"
date: 2025-10-22
decision-makers: Jaime Ochoa de Alda Cerdan and Alejandro Garcia Prada 
consulted:
informed:
---

# {short title, representative of solved problem and found solution}

## Context and Problem Statement

{Describe the context and problem statement, e.g., in free form using two to three sentences or in the form of an illustrative story. You may want to articulate the problem in form of a question and add links to collaboration boards or issue management systems.}

## Considered Options

* 0001-1-MCV
* 0001-2-Server-Client
* 0001-3-Event-Driven-Architecture

## Decision Outcome

Chosen option: Server-Client, because we need a distributed, scalable, and centralized platform where multiple clients (web, mobile, managers) can access the same business logic and database, ensuring security, integrity, and control from a single server. Another reason is the importance of the asyncronous calls.

### Consequences

* Good, because it satisfied all the requirements and has an horizontal scalability. 
* Bad, because the big server dependence and big initial complexity.

<!-- This is an optional element. Feel free to remove. -->
### Confirmation

{Describe how the implementation / compliance of the ADR can/will be confirmed. Is there any automated or manual fitness function? If so, list it and explain how it is applied. Is the chosen design and its implementation in line with the decision? E.g., a design/code review or a test with a library such as ArchUnit can help validate this. Note that although we classify this element as optional, it is included in many ADRs.}

<!-- This is an optional element. Feel free to remove. -->
## Pros and Cons of the Options

### 0001-1 MVC

The MVC model (Model–View–Controller) is a way to organize code in three parts:
Model: Handles the data and business logic.

View: Displays the information to the user.

Controller: Connects the Model and View, managing user input and updating the View as needed.

More info: https://developer.mozilla.org/es/docs/Glossary/MVC

* Good, because it's very simple and has been used largely.
* Good, because clear responsability separation.

* Bad, because limited scalability.
* Bad, because of the difficulties integrating asyncronous processes.


### 0001-2-Server-Client

 The Server–Client MVC model splits responsibilities between two main parts:

Client (View & Controller): The user’s side — it shows information (View) and sends user actions to the server (Controller logic can be partly here).

Server (Model & Controller): The backend — it processes requests, applies business logic (Model), and sends responses or data back to the client.

This setup makes web apps faster and more scalable, with the client handling the interface and the server managing data and logic.

More info: https://www.geeksforgeeks.org/system-design/client-server-architecture-system-design/

* Good, because {argument a}
* Good, because {argument b}
* Neutral, because {argument c}
* Bad, because {argument d}
* …

### 0001-3-Event-Driven-Architecture

 The Event-Driven Architecture (EDA) is a design pattern where the system reacts to events as they happen.
Instead of the client constantly asking the server for updates, the server emits events in real time, and the client listens for them.

Event: Something that happens — like a user moving on a map or new data being created.

Producer: The part that creates and sends the event.

Consumer: The part that receives and reacts to the event.

More info: https://reactiveprogramming.io/blog/es/estilos-arquitectonicos/eda

* Good, because {argument a}
* Good, because {argument b}
* Neutral, because {argument c}
* Bad, because {argument d}
* …
