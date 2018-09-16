# Versioning in an Event Sourced System

By Gregory Young.

## Why version?

## Why can't I update an event?
### Inmutability
### Consumers
### Audit
### Worms
### Crime

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