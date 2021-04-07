# TypeScript DDD Demo Application
**Note:** For use with RentSpree

This is a demo application based on my [Typescript DDD boilerplate project](https://github.com/yerinadler/typescript-ddd-demo-app)

## Context
This is a simple rental application in which a user can apply for a property he/she wants to rent by filling the application.

The application is simply a reference between the user and the property.

**Note:** Currently, there is no business logic to control how the users can apply for a property. Thus, the users can apply for whatever property they want 

## Architecture
This project uses DDD with Onion Architecture as illustrated in below images

![](https://res.cloudinary.com/practicaldev/image/fetch/s--5A11Acxs--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://img.barrymcauley.co.uk/onion_architecture.jpg)

It is important to note here that `the flow` of the dependencies is `inside-out` (The components in the outer circles can rely on the inner circles of the onion and not the other way around)

Thus, there is no such thing like `repository implementation` or `Seneca client implementation` leaked into the inner layer which is the domain layer. All must be done primarily in the `infrastructure` layer and delegated by the `application` layer

All of the dependencies between components are inverted using `Inversify` which is one of the most popular `IoC` Container for TypeScript.

### Components
**Note:** The description format is **[Component Name]** - **[Description]** **[Layer]**

Domain Driven Design (Tactical part) primarily focuses on building rich domain models. Thus, we should focus on it. However, in this project; based on the Onion Architecture, we introduce these components

* **Domain Entity** - Lives in the domain layer (`Property and User`). Identified with identity uniqeness **[Domain]**
* **Aggregate Root** - A domain entity that acts as an entry point to relevant child entities and value objects (See `Property` and `User` in the domain directory). All accesses must be performed through the aggregate root only **[Domain]**
* **Value Object** - An object which can be identified with structural equality (See Address in `@domain/application`) **[Domain]**
* **Domain Service** - Contains the business logic which doesn't fit into a single entity (See `@domain/application/services/ApplicationRegistration` which handles user application to the property). This is a bit tricky when because some of the domain services might require some external information via HTTP requests like Seneca for RentSpree. This way we define the contract in the domain layer and implement this service in the infrastructure layer instead. Otherwise the implementation details will leak directly into the domain layer which is not allowed conceptually **[Domain / Infrastructure]**
* **Repositories** - Handle `persistence` of the aggregate root. The repositories must return the aggregate root not the pure database result as opposed to the `ORM` or `Query Object` **[Infrastructure]**
* **Data Mapper** - Handle data transformation between the domain entities and database objects **[Infrastructure]**
* **Data Transfer Objects** - Data contracts between layers **[Any layer]**
## Technologies
1. Node.js
2. TypeScript
3. MongoDB with MongoDB native driver (mongodb package on NPM)
4. InversifyJS as an IoC container
5. Express (via Inversify Express Utils) as an API framework

## Getting Started
To run the project, make sure you have these dependencies installed on your system

1. Node.js v8 or later
2. Typescript with `tsc` command
3. Nodemon
4. ts-node
5. MongoDB (via MognoDB Native Driver)

You also need to setup and initialise MongoDB database. Then, copy the `.env_example` file into `.env` file by firing the command

````bash
cp .env_template .env
````

Do adjust the DB_NAME and MONGODB_URI to match your configuration then run

````bash
yarn dev
````

## Advancing the approach
Doing DDD by focusing on writing rich domain model is great. But one day you will find that segregating the read and the write model could yield better performance in the long run. That's why most of the times engineers / architects who implement `DDD` will also embrace `CQRS` and `Event Sourcing` in some of their projects to maximize `scalability` and `performance` of the system.

To see how this project can be evolved into, please check [another repository](https://github.com/yerinadler/typescript-event-sourcing-sample-app) of mine which implements `CQRS` and `Event Sourcing` on the top of this project. Cheers!

