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
