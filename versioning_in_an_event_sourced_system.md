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
### Define a version of an event
* A new version of an event must be convertible from the old version of the event. If not, it is not a new version of the event but rather a new event.

```c#
InventoryItemDeactivated_v2 ConvertFrom(InventoryItemDeactivated_v1 e) {
  return new InventoryItemDeactivated_v2(e.Id, "Before we cared about reason");
}
```
> The event could be passed through some converters that move it forward in terms of version. The code will no longer see an *InventoryItemDeactivated_v1* event, and the handler in the domain object can be removed.

* Unfortunately, using this style of serialization requires every node to actually have the schema of the event. If it does not have, it will not be able to even deserialize that event.

* Move the schema resolution to a central service.
    * They will request the schema on demand from a centralized service and apply it dynamically.
    * It can introduce significant complexity, need for a framework, and a possible availability problem to the system.

### Double write
* The new version of the producer write both the *_v1* and the *_v2* versions of the event when it writes. This is also known as Double Publish.
* It must only handle the version of the event you understand and ignore all others.
* The old version of the event will be deprecated. This gives subscribers time to catch up.
* It's not unusual in a distributed systems.
    * It works well in a stable situations of ES system.
    * It fails when you need to replay a projection.

* State hydrated from a stream to validate a command is a projection.

### But...
* Using types to specify schema forces to all consumers update to understand the schema before a producer is.

## Weak schema
### Mapping
* It's used something like json or xml, combined with what is known as weak-schema or hybrid-schema, to serialize their messages. 

* Exists on json and instance -> value from json
* Exists on json but not on instance -> NOP
* Exists on instance but not in json -> default value

* When using mapping, there is no longer an addition of a new version of the event. However:
    * Your are no longer allowed to rename something. You can deprecate it, but it is annoying operation as well.
    * There will often be programmatic checks to ensure what you expect to be in the message after the mapping is in fact present. 

### Wrapper
* Write/generate a wrapper over the serialized form.
    * Tends to be more work.
    * It requires some additional rules to be followed to deal with schema evolution.
    * It has some significant advantages.

* The class is not directly mapping to and from the json; it is instead wrapping it and providing typed access to it.
* If you are enriching something, you can move it forward without understanding everything in the message.
* If this json were mapped to a `Deactivated` event containing *Id*, *Reason*, and *AuthCode* and then serialized again, the *userId* would be lost in the mapping process.
    * By using a wrapper, the *userId* can be preserved even if it is not understood.

### Overall considerations
* For most systems, a simple human-readable format such as *JSON* or *XML* is fine for handling messages.
* The wrapper may be a solution for a lot of performance scenarios or to pass data without understand.

## Negotiation
* Some transports support bi-directional communications, which can be used to negotiate what format a message will arrive in.
* The most common of such transports in use is HTTP.
. 
### Atom
* It's designed for blogs.
* It's a common distribution method for event streams.
* The URIs and the pages are both immutable.
    * They can infinitely cached.
* The client is making a request so it can include a header to explain the version that it can understand.
* HTTP and Atom the most popular protocols thart support negotiation.

### Move negotiation forward
* Some message transports and middleware support it.
* A consumer register the content-types that there are interested.
* The consumer sends all of the content types.
* The publisher then converts a given message to the best match content type on its end before sending it to the consumer.
* Not needing to negotiate per message when changes only happen rarely.
* If using type-based schema, only the translation code needs to be updated.
* Consumers can be updated as needed and can still get the old version of the message they understand.
    * If a new field is to be added to an event, only the teams that need to use it need support for it.

### How to translate
* The translations can happen either using type-based schema or weak schema.
* In the type-based schema apart from the upcasting we need the downcasting.
* It can be preferred converting from v1 to v9 directly as opposed to performing nine conversions.
    * This approach is followed by weak schemas.
* If your conversions becomes more difficult, it might be worth considering depreciating old versions of events.

### Use with structural data
* Instead of converting the messages to different views, this service would instead replay all events up to a given event.
* It provides the immutability of URIs so, it allows the caching.
* The consumers will get a state per change, thereby not always getting the latest value, which can miss a change if these are multiple.
* The service responsible for generating the varying structural views of the state at a given point just has a few small in-memory projections to build the varying structural views and return them.

## General versioning concerns
### Versioning of behaviour
* How do I version behaviour of my system as my domain logic changes?
* If you find yourself putting branching logic or calculation logic in a projection, especially if it is based on time, you are probably missing logic in the creation of that event.

This code very quickly spirals out of control. 
```c#
public void Apply(ItemSold e) {
    if(e.Date < Convert.ToDateTime("1/1/2017")) {
        _tax = e.Subtotal * .18;
    } else {
        _tax = e.Subtotal * .19;
    }
}
```
Better if we use:
```c#
public void SellItem(...) {
    var tax = subtotal * .18;
    Apply(new ItemSold(_id,....,tax));
}
```
And then:
```c#
public void SellItem(...) {
    var tax = subtotal * .19;
    Apply(new ItemSold(_id,....,tax));
}
```

### Exterior calls
* The ideal work is to make call and store in the event the result.
```c#
public void SellItem(...) {
    var customerScore = _externalService.GetCustomerScoreFor(customer);
    Apply(new ItemSold(_id, ..., customerScore));
}
```
* There are times the cost of storing may be high or due to legal restrictions the information is not allowed to be stored with the event.
* The projection itself was replayable but from a user’s perspective the infomation was non-deterministic.
    * Contractual obligation.
* Another problem happens when projections start doing lookups to other projections.
* To avoid that, you can:
    * Share information between projections.
    * Consider the entire read model a single projection and replay the read model as a whole.
    * Place projections into Projection Groups for replay but this can easily be overlooked and should be looked at as a last resort.

* If you find a projection making calls to external services or to other projections it will be a problem to replay later.

### Changing semantic meaning
* Semantic meaning cannot change between versions of software.
* There is no good way for a downstream consumer to understand a semantic meaning change. 

### Snapshots
* Domain objects or state in a functional system can be rebuilt by replaying a stream of events.
* When the stream of events gets large, say 1000 events it is beneficial to begin to save the state of a replay at a given point.
* This allows for faster replays.
* Versioning snapshots have the same problems that they have versioninig structural data.
* The normal way of handling the problem is to rebuild the entire snapshot and to later delete the previously generated snapshot.
* Before remove the previous snapshots, they will be depreceated.
    * The software version dependent on the snapshots is removed first and once there is no longer anything using the snapshots, the snapshots can be deleted as well.
* It is often a good idea to keep older snapshots around for a while if only to allow the ability to downgrade to an older version of software in case of bugs etc. 

### Avoid "and"
* It's an anti-pattern that leads to temporal coupling and is likely to become a versioning issue in the future.
* Event Sourced systems have two sets of Use Cases:
    * Those you can tell the system to do, and
    * those the system has said it has done.
    * They do not always align.
* Do not use the word “And” in an event name; use two events instead.

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