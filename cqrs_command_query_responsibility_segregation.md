# CQRS (Command Query Responsibility Segregation)

By Ajay Kumar.

## Module 1: Introduction
### CQRS and its origins
* CQRS was introduced by Greg Young in 2010 based on the Bertrand Meyer's CQS idea.
* Command: produces side-effects and returns void.
* Query: side-effects free and returns non-void.
* CQS:
    * Method command
    * Method query
* CQRS:
    * Command model
    * Query model

### Why CQRS?
* Scalability. There are more reads than writes in a typical system, so it is important to scale independently.
* Performance. You can apply different solutions for read side and write side. * Simplicity. Apply SRP at the architectural level simplifying the models.

### CQRS in the real world
* If you used ORM for write and raw SQL for read.
* If you generate SQL views to optimize some queries.
* If you used full-text search engine to improve query performance.

## Module 2: Introducing a sample project
(Nothing relevant. It exposes CRUD mindset application that he is going to be refactorize in the next modules).

## Module 3: Refactoring towards a task-based interface
### CRUD-based interface
* Organize your application along create, read, update and delete guidelines.
* The complexity grows.
    * Too many features in a single method.
    * Increase in number of bugs.
* Disconnection between how the domain experts view the system and how it is implemented. Lack of ubiquitous language.
* Lack of unified language leads to maintainability issues.
    * Have to translate from experts' language.
    * Reduces capacity to understand the task.
* Damage the UX. The user has to investigate the interface on their own.

### Task-based interface
* It is the result of identifying each task the user can accomplish with an object in the application.
* It restores the SRP.
* It simplifies the code base.
* It improves the UX.

### Untangling de update method
* DRY = Domain knowledge duplication
    * Two classes coincide in their structure. Their duplication is not bad.
    * There is no domain knowledge duplication.

### Recap: untangling de update method
* Avoiod DTOs that theis fields are not used in 100% cases.
* They are strong sign you have a CRUD-based interface.

### Dealing with create and delete methods
* CRUD-based interface is just fine when your application is not too complex, or you are not going to maintain or evolve it in the future.

## Module 4: Segregating commands and queries
### Commands in CQS vs commands in CQRS
* In CQS all methods in a class should be either commands or queries.
* In CQRS it is an explicit representation of what needs to be done in the application.
* The concepts are interconnected.

### Commands and queries in CQRS
* Commands:
    * tell the application to do something.
    * imperative tense.
    * should use ubiquitous language.
* Queries:
    * ask the application about something.
    * start with "Get".
* Events:
    * inform external applications.
    * past tense.

### Commands and queries in the onion architecture
* It is someone else, not our application, that raises the commands.
    * That would be the push moodel.
* It is us, our application, that raises the events.
    * That is the so-called pull model.
* The classes inside the core domain should not refer to the outside world.

### Commands vs DTOs
* Commands and DTOs tackle different problems.
* Commands: serializable method calls.
* DTOs: data contracts and backward compatibility.
* To use commands as DTOs is the same that to use entities as DTOs.
    * Hinder refactoring.
* It is much easier to implement the mapping between the two than to try to lump all these reponsibilities into the same class.
* It is fine not to have DTOs if you do not need BC.
    * A single client which you develop yourself.
    * Can deploy both the API and the client simultaneously.

## Module 5: Implementing decorators upon command and query handlers
### Decorator pattern
* It is a class or a method that modifies the behaviour of an existing class or method without changing its public interface.
* It introduces cross-cutting concerns.
* It avoids code duplication.
* It adheres SRP.
* Command handlers manage business use cases and decorators technical issues.

### Command and query handlers best practices
* Put command and query handlers inside their respective commands and queries.
* Don't reuse command handlers.
    * Avoid code duplication creating domain services.

## Module 6: Simplifying the read model
### Separation of the domain model
* Same domain model for reads and writes:
    * Domain model overcomplication.
    * Bad query performance.
* There is no need for a domain model within the read side.
* No data modifications so:
    * No neeed in encapsulation.
    * No need in DDD.
    * No need in abstractions.
        * No need ORMs.
        * Can use database-specific features to optimize the performance.

### The read model and the onion architecture
* Queries do not have to use DTOs.
    * DTOs are infrastructure details.
* If queries are no part of the onion, also they are no part of the domain model.
* Domain contains:
    * Model.
    * Events.
    * Commands.
