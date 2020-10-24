# Release It!
>Design and Deploy Production-Ready Software

By Michael T. Nygard.

## Chapter 1. Living in Production
### Aiming for the Right Target
* We need to design individual software systems, and the whole ecosystem of interdependent systems, to operate at low cost and high quality.

### The Scope of the Challenge
* The increasing scope of this challenge —to build software fast that’s cheap to build, good for users, and cheap to operate— demands continually improving architecture and design techniques. Designs appropriate for small WordPress websites fail outrageously when applied to large scale, transactional, distributed systems, and we’ll look at some of those outrageous failures.

### A Million Dollars Here, a Million Dollars There
* Design and architecture decisions are also financial decisions.
* These choices must be made with an eye toward their implementation cost as well as their downstream costs.

### Use the Force
* Your early decisions make the biggest impact on the eventual shape of your system.
* The earliest decisions you make can be the hardest ones to reverse later.
* The beginning is when your team is most ignorant of the eventual structure of the software, yet that’s when some of the most irrevocable decisions must be made.

### Pragmatic Architecture
* The ivory-tower architect most enjoys an end-state vision of ringing crystal perfection, but the pragmatic architect constantly thinks about the dynamics of change.

* Software delivers its value in production. The development project, testing, integration, and planning...everything before production is prelude.

## Chapter 2. Case Study: The Exception That Grounded an Airline
* It’s just fantasy to expect every single bug like this one to be driven out. Bugs will happen. They cannot be eliminated, so they must be survived instead.
* How do we prevent bugs in one system from affecting everything else?

## Chapter 3. Stabilize Your System
### Defining Stability
* A robust system keeps processing transactions, even when transient impulses, persistent stresses, or component failures disrupt normal processing.
* The terms impulse and stress come from mechanical engineering.
* An impulse is a rapid shock to the system.
* Stress to the system is a force applied to the system over an extended period.

### Extending Your Life Span
* The major dangers to your system’s longevity are memory leaks and data growth.
* Both kinds of sludge will kill your system in production. Both are rarely caught during testing.

### Failure Modes
* You can decide what features of the system are indispensable and build in failure modes that keep cracks away from those features.

### Stopping Crack Propagation
* The more tightly coupled the architecture, the greater the chance this coding error can propagate.
* Conversely, the less-coupled architectures act as shock absorbers, diminishing the effects of this error instead of amplifying them.

### Chain of Failure
* Fault: A condition that creates an incorrect internal state in your software. A fault may be due to a latent bug that gets triggered, or it may be due to an unchecked condition at a boundary or external interface.
* Error: Visibly incorrect behavior. When your trading system suddenly buys ten billion dollars of Pokemon futures, that is an error.
* Failure: An unresponsive system. When a system doesn’t respond, we say it has failed. Failure is in the eye of the beholder...a computer may have the power on but not respond to any requests.
* Faults become errors, and errors provoke failures. That’s how the cracks propagate.

## Chapter 4. Stability Antipatterns
Assume the worst. Faults will happen. We need to examine what happens after the fault creeps in.

### Integration Points
* Every integration point will eventually fail in some way, and you need to be prepared for that failure.
* Prepare for the many forms of failure.
* Debugging integration point failures usually requires peeling back a layer of abstraction. Failures are often difficult to debug at the application layer because most of them violate the high-level protocols.
* Failure in a remote system quickly becomes your problem, usually as a cascading failure when your code isn’t defensive enough.

### Chain Reactions
* Recognize that one server down jeopardizes the rest.
* The increased traffic means they leak memory faster.
* Hunt for obscure timing bugs.
* As long as the scaler can react faster than the chain reaction propagates, your service will be available.
* Defend with Bulkheads.

### Cascading Failures
* Stop cracks from jumping the gap.
* Scrutinize resource pools.
* Defend with Timeouts and Circuit Breaker.

### Users
* Users consume memory.
* Users do weird, random things. Look into fuzzing toolkits, property- based testing, or simulation testing.
* Malicious users are out there. Keep your frameworks up-to-date, and keep yourself educated.
* Users will gang up on you. Run special stress tests to hammer deep links or hot URLs.

### Blocked Threads
* Recall that the Blocked Threads antipattern is the proximate cause of most failures.
* Like Cascading Failures, the Blocked Threads antipattern usually happens around database connection pools.
* Any library of concurrency utilities has more testing than your newborn queue.
* Avoid infinite waits in function calls; use a version that takes a timeout parameter.
* Acquire and investigate the code for surprises and failure modes.

### Self-Denial Attacks
* Create static “landing zone” pages for the first click from these offers. Watch out for embedded session IDs in URLs.
* Programming errors, unexpected scaling effects, and shared resources all create risks when traffic surges.
* Anybody who thinks they’ll release a special deal for limited distribution is asking for trouble.

### Scaling Effects
* Examine production versus QA environments to spot Scaling Effects.
* Watch out for point-to-point communication. Maybe replace with some kind of one-to-many communication.
* If your system must use some sort of shared resource, stress- test it heavily.

### Unbalanced Capacities
* Check the ratio of front-end to back-end servers, along with the number of threads each side can handle in production compared to QA.
* Observe near Scaling Effects and users.
* Try test cases where you scale the caller and provider to different ratios. You should be able to automate this all through your data center automation tools.
* Stress both sides of the interface.

### Dogpile
* They force you to spend too much to handle peak demand.
* Don’t set all your cron jobs for midnight or any other on-the-hour time.
* Use a backoff algorithm so different callers will be at different points in their backoff periods.

### Force Multiplier
* Build limiters and safeguards into them so they won’t destroy your whole system at once.
* Actions initiated by automation take time. That time is usually longer than a monitoring interval, so make sure to account for some delay in the system’s response to the action.

### Slow Responses
* Slow Responses trigger Cascading Failures.
* For websites, Slow Responses cause more traffic.
* Fail fast.
* Hunt for memory leaks or resource contention.

### Unbounded Result Sets
* Typical development and test data sets are too small to exhibit this problem. You need production-sized data sets to see what happens when your query returns a million rows that you turn into objects.
* Paginate at the front end.
* Don’t rely on the data producers.
* Put limits into other application-level protocols.