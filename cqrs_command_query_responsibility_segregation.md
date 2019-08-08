# CQRS (Command Query Responsibility Segregation)

By Ajay Kumar.

## Module 1: Introduction
### CQRS and its origins
* CQRS was introduced by Greg Young in 2010 based on the Bertrand Meyer CQS idea.
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
