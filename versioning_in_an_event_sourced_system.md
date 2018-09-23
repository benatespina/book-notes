# Versioning in an Event Sourced System

By Gregory Young.

## Why version?
* Many developers run into issues dealing with versioning, particularly in Event Sourced systems.
* ES systems are easier to version than structural data in most instances.

## Why can't I update an event?
* *How do I write these two documents in a transaction?* is a wrong defined question because,
    * If you are trying to update multiple documents in a transaction, it most likely means that your model is wrong. It has many trade-offs in terms of transaction coordination.
* *Why can't I update an event?* 
    * If you edit an existing event in your system, in reality, there are many circumstances where you should avoid it at all costs.

### Immutability
* Systems gain in terms of simplicity and scalability.
* With that the *cache-invalidation* issue goes away.
    * Something immutable is infinitely cacheable.

### Consumers
* If you edit an event, how will the consumers of that event be notified that the event has changed?
    * What if you had a projection that was interested in this event? It had received it previously, and had updated a column in a SQL database because of the event. If and when you changed the event, how would it be notified of the change that had occurred?

### Audit
* A large number of ES systems are Event Sourced specifically because they need an audit trail.
    * If you can edit your audit trail, it's not an audit trail.
* The moment you allow a single edit of an event, maintaining a proper audit log becomes impossible.
* Immutability is immutable. The moment you allow a single edit, everything becomes suspect.

### Worms
* Write once, read many.
* The use of WORM drives is not only focused on the *auditability* of a system but also on its *security*.

### Crime
Whether we talk about fraud or embezzlement, there are a vast number of existing systems where the concept of editing an event is an impossibility.

## Basic type based versioning
### Getting started
### Define a version of an event
### Double write
### But...

## Weak schema
### Mapping
### Wrapper
### Overall considerations

## Negotiation
### Atom
### Move negotiation forward
### How to translate
### Use with structural data

## General versioning concerns
### Versioning of behaviour
### Changing semantic meaning
### Snapshots
### Avoid "and"

## Whoops, I did it again
### Accountants use pens
### Types of compensating actions
### How do I find what needs fixing?
### More complex example
### But I really screwed up

## Copy and replace
### Simple copy-replace
### Simple>
### With a system live
### Stream boundaries are wrong
### Changing stream boundaries on a running system

## Cheating
### Two aggregates, one stream
### One aggregate, two streams
### Copy-transform
### Versioning bankrupty

## Internal vs external models
### External integrations
### Granularity
### Rate of change
### How to implement
### Summary

## Versioning process managers
### Process managers
### Basic versioning
### Upcasting state
### Take over
### Warning