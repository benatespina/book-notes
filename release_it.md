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