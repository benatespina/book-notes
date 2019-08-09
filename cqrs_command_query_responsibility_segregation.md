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
