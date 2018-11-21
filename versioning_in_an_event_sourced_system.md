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
* ES system works like a company.
* For instance, if the accountant accidentally transfers a wrong quantity he doesn’t erase operation in the middle of their ledgers; instead of that, they will add new transactions to compensate for the one in error.

Let’s imagine that the account makes a fat-finger mistake and accidentally transfers $10,000 instead of $1,000 from Account A to Account B.

* *Partial reversal*: make a new transaction restoring the wrong quantity between accounts. 

|ID | From      | To        | Amount  | Reason   |
|:-:|:---------:|:---------:|:-------:|:--------:|
|13 | Account A | Account B |  $10000 | NONE     |
|27 | Account B | Account A |  $9000  | ERROR 13 |

* *Full reversal*: make the previous transaction but in an inverse way, and then make the correct transaction.

|ID | From      | To        | Amount  | Reason   |
|:-:|:---------:|:---------:|:-------:|:--------:|
|13 | Account A | Account B |  $10000 | NONE     |
|27 | Account B | Account A |  $10000 | REV 13   |
|29 | Account A | Account B |  $9000  | NONE     |

### Types of compensating actions
* Implement the opposite use case that restore the state to the previous stage.
* Create these events ad-hoc when/if they occur.
    * Many Event Stores support the ability to write an event to a stream ad-hoc, say, from a json file with curl or from a small script.
    * Projections do not yet understand this event. In such a case, you would need to update that consumer and then put in the ad-hoc correction.
* Introduce a special type of event into your system known as a *Corrected event* or a *Cancelled event*.
    * This event would contain a link to the original event that it was cancelling. 

### How do I find what needs fixing?
* Bring up a one-off instance of the domain model that will iterate through all of the “possibly affected aggregates one by one, emitting the compensation as it finds domain objects that may be affected.
* Every projection is ephemeral. There is nothing wrong with bringing one up solely to determine what things have been affected.
* The state your domain model uses, is itself a projection.
* Write a one-off projection, analyze your data, and determine what the scope of your change will be.
    * Understand it, discuss its consequences, and finally, if you determine it’s still the right move, run the compensating actions.

### More complex example
* The reading back to the 0 cross allows the system to not maintain intermediate snapshots of what was in the position at given points as it can recalculate it from that point.
    * Unless the number of trades between 0 crosses is very high this is generally preferable.

### But I really screwed up
* Event store implementations offer ways of deleting streams.
* It's a safe operation, whereas deleting a single event or editing an event is not.
* Deleting from the beginning of a stream is also a safe operation.
* Sometimes we have data retention legislation that says we can only keep X information for T time.
    * You can be handled with stream deletion, deleting the events specifically past a certain age. 
    * You can keep two streams, one with private data and the other one with the public data.
    * If you allow consumers to retain such information, you should also put a `RemovedPrivateData` event so that any consumers can also be notified that they should no longer retain the information.

## Copy and replace
* It's a pattern to:
    * Merge two events into one. 
    * Split one event into two.
    * Update an event.
 
### Simple copy-replace
* The general idea is to read the events out of the old stream and write them to a new one.
* During this process you can:
    * To get rid of events, they would just not be copied.
    * To transform events from one version to another, they could be upcast.
    * Any merging and splitting of events can be done easily.
    * Any transformation can sit in the middle.
* After the process, the original stream is then deleted.

### In place copy-replace
* Event Store must supports *truncate before* operation.
* Instead of writing to a new stream, the events are appended back to the end of the same stream.

### Simple>
* Copy-Replace is the nuclear-option of versioning.

### Consumers
* Include same identifier on the new event as the one you are copying.
* You can’t assign the same identifier to both of the split events.
* Copy-Replace is executed in an offline manner.
    * An administrator stops incoming transactions, runs the Copy-Replace, re-runs all projections for read models, and brings the system back up.

### With a system live
* If *Copy-Replace* is run under such circumstances, the old events are transformed and appended to the log as new events.
    * This implies that things such as projections will see them as new events.
* If in the time it took to process the command, the Copy-Replace occurred, and now the stream is gone.
* There are some strategies but,
    * Not all work in all scenarios.
    * Not all scenarios are solvable.
* Both the old and new versions of the software be able to understand both the new and old versions of the stream.
* Instead of deleting the old stream after *Copy-Replace*, write an event at the end of the stream saying this stream has been migrated.
* For auditing purposes, it can be worthwhile to put a pointer event pointing back to the original stream instead of deleting it.
* The Copy-Replace will run asynchronously while the system is running.
    * During the migration process, the producer will, when loading/writing to the stream, first check the last event in it.
    * If it's a pointer event, it will follow that link and use the new stream.
    * “If it is not, it will remember the version number of the event it read and use it as *ExpectedVersion*.
* Some types of transformations may be occuring:
    * Safe (Upgrade Version of Event, Add New Event).
    * Dangerous (Split Event, Merge Events) but can be worked around.
    * Very dangerous (Delete Event, Update event).

* Let the read models go out of sync briefly and then rebuild/replace asynchronously.
    * If this is done a short period of time after the problem arises, the damage can often be limited and/or turned into a customer service issue.
    * If it's not, you have to write an *invalidated event* before the pointer event.
* A projection will now first receive an Invalidated for the stream, to clean up required to invalidate any information it has previously received for this stream.
* After, the projection will then receive the entire new stream and simply process it same as it would any other new data.
* Copy-Replace can be a powerful tool.
    * It allows for many things you couldn’t do otherwise, including updating an event.
    * Its power has a real cost in terms of complexity. 

### Stream boundaries are wrong
* One of the worst situations people eventually run into with an ES system is that they modeled their streams incorrectly.
    * Because “the requirements of the system having changed over time.
    * Because developers make mistakes in analysis.
* For instance:
    * The system manages the maintenance of trucks.
    * Originally, Engine was modeled as part of a Truck.
    * Later, as the system matures and brings in more use cases.
    * Trucks and engines do not share a life cycle
    * They should be in different streams.
* Such as *Copy-Replace*, *Split-Stream* involves reading up from the first stream and writing down to two new ones.
* Avoid all transformation and rely on idempotency.
* If the two new streams will share some events.
    * The same event gets written to both streams.
    * This may cause problems with idempotency depending on the Event Store.
* *Join-Stream* is the opposite of *Split-Stream* and is handled in the same way.

### Changing stream boundaries on a running system
* *Split-Stream* and *Join-Stream* require both versions of software to support them.
* If the current version does not support both models, you will need to do an intermediate release to ensure it does.
* The strategy for *Split-Stream*:
    * Try to read the last event from the single stream.
    * If it is not a *StreamMovedTo* event, then continue to read the stream as normal.
    * Otherwise, follow the appropriate link in the *StreamMovedTo*. 
* The strategy for *Join-Stream*:
    * Go to the original stream.
    * Check the last event if it is a *StreamMovedTo*.
    * Follow the link; otherwise, continue with that stream.
* Use *ExpectedVersion* when writing back to the stream.
    * Stream may get migrated while you are working on it.
    * *ExpectedVersion* should be set to the last event read from the stream.
    * If this step is forgotten, you may write an event to a stream after it is migrated and lose it in the process. 

## Cheating
### Two aggregates, one stream
* There is nothing wrong with having two aggregates built off of the same stream.
* The setting of *ExpectedVersion* and having two aggregates can lead to more optimistic concurrency problems at runtime.
* It's a pattern that it does the same that *Split-Stream*.
* It is just doing it dynamically at the application level as opposed to doing it at the storage level.

### One aggregate, two streams
* It uses a join operation dynamically to combine two streams worth of events into a single stream that is then used for loading the aggregate.
* Most frameworks are centered around stream per aggregate.
* Instead of tracking *Event*, track something that wraps *Event* and also contains the stream that the *Event* applies to.
* It is quite common that a system need to store information though some of it must be deleted after either a time period or a user request.
    * Create two streams and have one aggregate running off of the join of the two streams.
* It's less common than multiple aggregates on the same stream pattern.
* It's useful when you need to do a *Join-Stream* at the storage level.

### Copy-transform
* An Event Store is a distributed log.
* Instead of migrating system to system in Copy-Transform you migrate version to version.
* It's like *Copy-Replace* but on the entire Event Store not just on a stream.
* You can rename an event, split one in two events, joins streams and so on in the transformation step like *Copy-Replace*.
* The new system
    * It has its own Event Store
    * it has all of its own projections to read models hooked to it.
    * As the data enters the Event Store it is then pushed out to the projections that update their read models.
    * The Event Store may catch up before the projections. It is very important to monitor the projections so you know when they are caught up.
    * Once they are caught up the system as a whole can do a *BigFlip*.
* *BligFlip* tells to the Event Store of the old system that it should stop accepting writes, drain all the writes that happened before.
    * It issues a special command to the old system at this point that will write a marker event.
    * This marker event identifies when the new system has caught up with the old system and can be updated to point all traffic at the new system.
* It removes most of the pain of trying to version a live running system by falling back to a *BigFlip*.
* It's simpler that trying to upgrade a live running system.

### Versioning bankrupty
* It's a specialized form of *Copy-Transform*.
* *How do I migrate off of my old RDBMS system to an Event Sourced system?*
    * To do a reverse engineering of the entire transaction history of given concepts from the old system, to recreate a full event history in the new system.
    * Create an *Initialized* event as starting point of the *Aggregate*.
* *How do I migrate off of my old ES system to a new ES system?*
    * It's easy to reverse engineer your history since it is well your history.
    * The *Initialized* event “allows you to truncate your history at a given point.
* To execute the bankrupty,
    * Run the migration then instead of deleting your old store and read models such as in Copy-Transform, keep them.
    * Label them for that time period. They are now there for reporting purposes if needed.
* It's difficult to analyze historical data outside of a single time period.
* This pattern does not work well in continuous systems.

## Internal vs external models
### External integrations
* It conforms the system to an existing standardized integration model.
* They can allow for modularity between systems.
* They can allow other systems to easily integrate with your system.

### Granularity
* Internal and external models have drastically different needs.
    * They have different granularity.
* Internal models tend to have very fine-grained events. Due to,
    * the separation between services.
    * the focus on a use-case being represented by an event.
* With internal models the service boundaries chosen for the system tend to be visible within the events in the model.
```
OrderPlaced: {
    orderId: 8282,
    customerId: 4432,
    products: [5757, 3321],
    total: 17.95
}
```

* External models generally prefer more coarse-grained events.
* An external model will generally denormalize relevant information onto the event.
    * It only wants to listen to a single event and to be able to act upon it
* An external consumer prefers to not have to listen to multiple events with storing intermediary information.
* This approach is more simple for an outside consumer.
* It provides a level of indirection from the internal model.
* It hides the details of how the internal model works.
* It's free to change its service boundaries or how it handles its internal eventing without affecting the external consumers.
```
OrderPlaced: {
    orderId: 8282,
    customer: {
        customerId: 4432,
        customerName: 'John Doe',
        postalCode: 'EC1-001',
        customerAge: 35,
        customerStatus: 'Gold'
    }
    products: [
        {name: 'razor blade ice cream', productId: 6565},
        {name: 'thing you should never eat', productId: 4242}
    ],
    total: 17.95
}
```

### Rate of change
* Changes to external models will possibly affect all consumers of the external model. For this reason,
    * external models are more formalized than internal models
    * changes to external models generally go through further diligence than changes to internal models.
* External models are designed keep in mind extensibility.
    * They are formalized to media types or described in a schema language.
    * Interactions will generally be well documented for consumers.
* Many external models are built using weak-schema.
    * Adding things to the external model will not break things. 

### How to implement
* The two models have different purposes and have quite different levels of granularity.
* A common implementation to introduce an external model is to introduce a service that acts as a Message Translator.
* All of the code associated with the translation to the external model is located in a single service.
    * The rest of the services know nothing of the external model.
* It's easy to introduce multiple of these services to support multiple external models.
 
### Summary
* Smaller systems can usually get away with only using an internal model.
* As the system size and complexity grows however it is often needed to introduce levels of indirection via the introduction of an external model.
* External models are often used even to other services maintained by the same team, the distintion is how the model is used not who uses it.
* Internal models tend to be very fine-grained.
* External models are more coarse.
* External models tend to be much more conservative in how they approach change compared to internal models.
    * A common pattern is to introduce an external model to allow the internal model to be more agile in regards to change.

## Versioning process managers
### Process managers
### Basic versioning
### Upcasting state
### Take over
### Warning