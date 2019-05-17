# Microservices AntiPatterns and Pitfalls

By Mark Richards.

## Chapter 1. Data-driven migration antipattern
* It occurs mostly when you are migrating from a monolithic application to a microservices architecture.

### Too many data migrations
* Data migrations are complex and error-prone—much more so than source code migrations.
* Understanding the risks involved with data migration and the importance of "data over functionality" is the first step in avoiding this antipattern.

### Functionality first, data last
* Migrate the functionality of the service first, and worry about the bounded context between the service and the data later.
* This technique avoids costly and repeated data migrations and makes it easier to adjust the service granularity when needed.

## Chapter 2. The timeout antipattern
* Service availability is the ability of the service consumer to connect with the service and be able to send it a request, as shown in 
* Service responsiveness, on the other hand, is the time it takes for the service to respond to a given request once you’ve communicated with it.

### Using timeout values
* Calculate the database timeout within the service and use that as a base for determining what the service timeout should be.
* Calculate the maximum time under load and double it, thereby giving you that extra buffer in the event it sometimes takes longer.
* These technniques generate high latency that usually is not assumbible from client side.

### Using the circuit breaker pattern
* It detects that a service is not responding, it will open, rejecting requests to that service. Once the service becomes responsive, the breaker will close, allowing requests through.
* The significant advantage of the circuit breaker pattern over timeout values is that the service consumer knows right away that the service has become unresponsive rather than having to wait for the timeout value.
* Circuit breakers can monitor the remote service in several ways:
	* Heartbeat check on the remote service (e.g., ping).
	* Synthetic transaction: a fake transaction is periodically sent to the service (e.g., once every 10 seconds).

## Chapter 3. The "I Was Taught to Share" antipattern
* Microservices is known as a “share-nothing” architecture.
    * Pragmatically, it is “share-as-little-as-possible” architecture because there will always be some level of code that is shared between microservices.
* It has a great countpart when every service is dependent on multiple custom shared libraries.

### Too many dependencies
* The more dependencies you have between services, the harder it is to isolate service changes, making it difficult to separately test and deploy individual services.

### Techniques for sharing code
* Shared project makes it easy to change and develop software.
    * Communication and control. It is difficult to know what shared modules changed and why, and also hard to control whether you want that particular change or not.
* Shared library makes development more difficult because for each change made to a module in a shared library, the developer must first create the library, then restart the service, and then retest.
    * Libraries can be versioned, providing better control over the deployment and runtime behavior of a service.
* Replication may seem risky, it avoids dependency sharing and preserves the bounded context of a service.
* Service consolidation. Since all of the services must be tested and deployed with the common module change anyway, you might as well just consolidate the functionality into a single service, thereby removing the dependent library.

## Chapter 4. Reach-in reporting antipattern
### Issues with microservices reporting
* Database pull model. Fast and easy but it leads to significant interdependencies between services and the reporting service.
    * It couples applications together through a shared database.
* HTTP pull model. Service makes a rest HTTP call to each service, asking for its data.
    * It is too slow for complex and huge data reporting.
* Batch pull model. If the service database schema changes, so must the batch data upload process.

### Asynchronous event pushing
* Event based pull model.
* It relies on asynchronous event processing to make sure the reporting database has the right information as soon as possible.
* It is relatively complex to implement.
* It requires a contract between each microservice and the data capture service for the data it is asynchronously sending, but that contract is separate from the database schema owned by the service.

## Chapter 5. Grains of sand pitfall
* Service granularity can impact performance, robustness, reliability, change control, testability, and even deployment.
* A service component is a component of the architecture that performs a specific function in the system.
* It should have a clear and concise roles and responsibility statement and have a well-defined set of operations.
* The number of implementation classes should not be a defining characteristic for determining the granularity of a service.
* There are three basic tests:
    * the service scope and functionality
    * the need for database transactions
    * the level of service choreography

### Analyzing service scope and function
* The right level of granularity is about the scope and function of the service.

### Analyzing database transactions
* Microservices architecture are distributed and deployed as separate applications.
* It is extremely difficult to maintain an ACID transaction between two or more remote services.
* They rely on a technique known as BASE transactions (basic availability, soft state, and eventual consistency).
* If you find you are constantly battling issues surrounding ACID vs. BASE transactions and you need to coordinate multiple updates, chances are you have made your services too fine-grained.

### Analyzing service choreography
* It refers to the communication between services.
    * Inter-service communication.
* It decreases the overall performance of your application since each call to another service is a remote call.
* Too much service choreography is that it can impact the overall reliability and robustness of your system.

## Chapter 6. Developer without a cause pitfall
### Understanding business drivers
* Understanding the business drivers behind choosing microservices is the key to avoiding this pitfall.
    * The reason for moving to microservices is to achieve better time to market via an effective deployment pipeline.
    * The reason for moving to microservices is to increase the overall reliability and robustness of the application.

## Chapter 7. Jump on the bandwagon pitfall
### Advantages and disadvantages
* Deployment, testability and change control, modularity, scalability, organizational change, performance, reliability, devOps.

### Other architecture patterns
* Service-Based Architecture
* Service-Oriented Architecture
* Layered Architecture
* Microkernel Architecture
* Space-Based Architecture
* Event-Driven Architecture
* Pipeline Architecture

## Chapter 8. The static contract pitfall
### Header versioning
* Protocol-aware contract versioning because the information about the version of the contract you are using is contained within the header of the remote access protocol.
* Regardless of the messaging standard, the version property is a string value that needs to match exactly with what the service is expecting, including being case-sensitive. For this reason it’s generally not a good idea to supply a default version if the version number cannot be found in the header.

### Schema versioning
* Protocol-agnostic contract versioning because the version identification is completely independent of the remote access protocol.
* The big advantage of this technique is that the schema (including the version) is independent of the remote access protocol.
* But a lot of disadvantages:
    * You must parse the actual payload of the message to extract the version number.
    * The schemas can get quite complex, making it difficult to do automated conversions of the schema.

## Chapter 9. Are we there yet pitfall
### Measuring latency
* Measuring the remote access latency under load in your production environment (or production-like environment) is critical for understanding the performance profile of your application.

## Chapter 10. Give it a rest pitfall
* There are two types of messaging standards you should be aware of when considering using messaging for your microservices architecture.
    * Platform-specific standards and platform-independent standards: JMS and MSMQ.
    * Platform-independent standard: AMQP.

### Asynchronous requests
* The first consideration for using messaging within your microservices architecture is asynchronous communication.
    * With Asynchronous requests the service caller does not need to wait for a response from the service when making a request.
* Not only does asynchronous processing increase overall performance, but it also adds an element of reliability to your system.
* Performance is increased because callers don’t have to wait for a response if none is needed.

### Broadcast capabilities
* It is not available within REST is the capability to broadcast a message to multiple services.

### Transacted requests
* The messages are sent to multiple queues or topics within the context of a transaction, the messages are not actually received by the services until the sender does a commit on that transaction.
* It is a good idea to consider using transacted messaging any time a service consumer needs to orchestrate multiple remote requests.