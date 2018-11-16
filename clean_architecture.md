# Clean Architecture
>A Craftsman's Guide to Software Structure and Design, First Edition

By Robert C. Martin.

## I. Introduction
When software is done right, it requires a fraction of the human resources to create and maintain. Changes are simple and rapid. Defects are few and far between. Effort is minimized, and functionality and flexibility are maximized.

This is a bit utopian vision.

We have seen coupled systems that every change, regardless of how trivial, takes weeks and involves huge risks. Also, we have seen bad designed code that it has huge negative effect on the morale of the team, the trust of the customers, and the patience of the managers.

### 1. What is design and architecture?
Architecture is used at a high level. that is divorced from the lower-level
Design seems to imply structures and decisions at a lower level.

But it's not true. The low-level details and the high-level structure are all part of the same whole.

#### The goal?
The goal of software architecture is to minimize the human resources required to build and maintain the required system.

#### Conclusion
The best option is for the development organization to recognize and avoid its own overconfidence and to start taking the quality of its software architecture seriously.

### 2. A tale of two values
Every software system provides two different values to the stakeholders:

* *Behavior*: developers write the code that causes the stakeholder’s machines to satisfy those requirements.
* *Architecture*: software was intended to be a way to easily change the behavior of machines. When the stakeholders change their minds about a feature, that change should be simple and easy to make. The difficulty in making such a change should be proportional only to the scope of the change, and not to the shape of the change.

#### The greater value
If you give me a program that works perfectly but is impossible to change, then it won’t work when the requirements change, and I won’t be able to make it work. Therefore the program will become useless.

If you give me a program that does not work but is easy to change, then I can make it work, and keep it working as requirements change. Therefore the program will remain continually useful.

For business managers, the current functionality is more important than any later flexibility. In contrast, if the they ask you for a change, and your estimated costs for that change are unaffordably high, they will likely be furious that you allowed the system to get to the point where the change was impractical.

#### Eisenhower's matrix
* Behavior is urgent but not always particularly important.
* Architecture is important but never particularly urgent.

The right order should be:
1. Urgent and important
2. Not urgent and important
3. Urgent and not important
4. Not urgent and not important

The mistake is to elevate items in position 3 to position 1, ignoring the important architecture of the system in favor of the unimportant features of the system.

It is the responsibility of the software development team to assert the importance of architecture over the urgency of features.

#### Fight for the architecture
The development team has to struggle for what they believe to be best for the company, and so do the management team, and the marketing team, and the sales team, and the operations team.

As a software developer, you are a stakeholder. You have a stake in the software that you need to safeguard.

If architecture comes last, then the system will become ever more costly to develop, and eventually change will become practically impossible for part or all of the system. If that is allowed to happen, it means the software development team did not fight hard enough for what they knew was necessary.

## II. Starting with the bricks: programming paradigms
* Paradigms are ways of programming, relatively unrelated to languages.
* A paradigm tells you which programming structures to use, and when to use them.

### 3. Paradigm overview
* Structured programming imposes discipline on direct transfer of control.
* Object-oriented programming imposes discipline on indirect transfer of control.
* Functional programming imposes discipline upon assignment.

None of them adds new capabilities. Each imposes some kind of extra discipline that is negative in its intent. The paradigms tell us what not to do, more than they tell us what to do.

They are align with the three big concerns of architecture: *function*, *separation of components*, and *data management*.

### 4. Structured programming
#### Proof
* Certain uses of goto statements prevent modules from being decomposed recursively into smaller and smaller units.
* Good uses of goto corresponded to simple selection and iteration control structures such as if/then/else and do/while. Modules that used only those kinds of control structures could be recursively subdivided into provable units.

All programs can be constructed from just three structures: sequence, selection, and iteration.

The very control structures that made a module provable were the same minimum set of control structures from which all programs can be built.

#### A harmful proclamation
Dijkstra wrote a letter "Go To Statement Considered Harmful". His point of view won.

As computer languages evolved, the goto statement moved ever rearward, until it all but disappeared. Most modern languages do not have a goto statement.

#### Functional decomposition
Structured programming allows modules to be recursively decomposed into provable units, which in turn means that modules can be functionally decomposed. That is, you can take a large-scale problem statement and decompose it into high-level functions. Each of those functions can then be decomposed into lower-level functions, ad infinitum.

#### No formal proofs
Programmers at large never saw the benefits of working through the laborious process of formally proving each and every little function correct.

#### Science to the rescue
Science does not work by proving statements true, but rather by proving statements false.

* Mathematics is the discipline of proving provable statements true.
* Science is the discipline of proving provable statements false.

#### Tests
*Testing shows the presence, not the absence, of bugs.* In other words, a program can be proven incorrect by a test, but it cannot be proven correct.

#### Conclusion
* It is the ability to create falsifiable units of programming that makes structured programming valuable today.
* Modern languages do not typically support unrestrained goto statements.
* We still consider functional decomposition to be one of our best practices.

### 5. Object-oriented programming
* The combination of data and function.
* A way to model the real world.

To explain the nature of OO we have: *encapsulation*, *inheritance*, and *polymorphism*.

#### Encapsulation?
It's a line that can be drawn around a cohesive set of data and functions. Outside of that line, the data is hidden and only some of the functions are known.

* *C* programming language has a perfect encapsulation.
    * Declare data structures and functions in header files.
    * Then, implement them in implementation files.
* *C++* broke the perfect encapsulation.
    * It needed the member variables of a class to be declared in the header file of that class.
    * It was partially repaired with visibility, but it was a hack for the compiler.
* *Java* and *C#* simply abolished the header/implementation split altogether.

So, it is difficult to accept that OO depends on strong encapsulation. Indeed, many OO languages have little or no enforced encapsulation.

#### Inheritance?
It's the redeclaration of a group of variables and functions within an enclosing scope.

#### Polymorphism?
It's an application of pointers to functions.

OO languages may not have given us polymorphism, but they have made it much safer and much more convenient. It makes polymorphism trivial.

OO imposes discipline on indirect transfer of control.

#### Conclusion
* OO is the ability, through the use of polymorphism, to gain absolute control over every source code dependency in the system.
* It allows the architect to create a plugin architecture, in which modules that contain high-level policies are independent of modules that contain low-level details.
* The low-level details are relegated to plugin modules that can be deployed and developed independently from the modules that contain high-level policies.

### 6. Functional programming
* The concepts predate programming itself.
* This paradigm is strongly based on the l-calculus.

#### Immutability and architecture
* Race conditions, deadlock conditions, and concurrent update problems are due to mutable variables.
* If you have infinite storage and infinite processor speed.
* If not, immutability is also viable but, if certain compromises are made.

#### Segregation of mutability
The first compromise is to segregate the application, or the services within the application, into mutable and immutable components.

* The immutable components perform their tasks in a purely functional way, without using any mutable variables.
* The immutable components communicate with one or more other components that are not purely functional, and allow for the state of variables to be mutated.

It's our responsibility to process as much as possible into the immutable components, and to drive as much code as possible out of those components that must allow mutation.

#### Event sourcing
This kind of architecture acts in a functional way. It does not storage the state.

If we have enough storage and enough processor power, we can make our applications entirely immutable—and, therefore, entirely functional.

#### Conclusion
* Structured programming is discipline imposed upon direct transfer of control.
* Object-oriented programming is discipline imposed upon indirect transfer of control.
* Functional programming is discipline imposed upon variable assignment.
* Software is not a rapidly advancing technology.
* The tools have changed, and the hardware has changed, but the essence of software remains the same.
* The stuff of computer programs is *composed of sequence*, *selection*, *iteration*, and *indirection*.

## III. Desing principles
SOLID principles tell us how to arrange our functions and data structures into coupled grouping of functions and data, and how those groups should be interconnected.

The goal of SOLID principals is the creation of software structures that:
* Tolerate change,
* Are easy to understand, and
* Are the basis of components that can be used in many software systems.

### 7. SRP: The single responsibility principle
It's a principle that has been commonly misunderstood so, there are some definitions that they have been used over the years.

* A module should have one, and only one, reason to change.
* A module should be responsible to one, and only one, user or stakeholder.
* A module should be responsible to one, and only one, actor.

#### Symptom 1: accidental duplication
The SRP says to separate the code that different actors depend on.

#### Symptom 2: merges
The way to avoid this problem is to separate code that supports different actors.

#### Solutions
* Move each function to the its own class.
* You can also, use a Facade pattern to group the classes.
* Each class contains a single public method but it can have all the private methods that it needs.
    * Each of the classes that contain such a family of methods is a scope. Outside of that scope, no one knows that the private members of the family exist.

#### Conclusion
The principle is about functions and classes, but,
* At the level of components, it becomes the Common Closure Principle.
* At the architectural level, it becomes the Axis of Change responsible for the creation of Architectural Boundaries.

### 8. OCP: The open-closed principle
A software artifact should be open for extension but closed for modification.

#### A thought experiment
Architects separate functionality based on how, why, and when it changes, and then organize that separated functionality into a hierarchy of components.

Higher-level components in that hierarchy are protected from the changes made to lower-level components.

#### Information hiding
Transitive dependencies are a violation of the general principle that software entities should not depend on things they don’t directly use.

#### Conclusion
* It's one of the driving forces behind the architecture of systems.
* The goal is to make the system easy to extend without incurring a high impact of change.
    * It's accomplished by partitioning the system into components, and arranging those components into a dependency hierarchy that protects higher-level components from changes in lower-level components.

### 9. LSP: The Liskov substitution principle
What is wanted here is something like the following substitution property: If for each object o1 of type S there is an object o2 of type T such that for all programs P defined in terms of T, the behavior of P is unchanged when o1 is substituted for o2 then S is a subtype of T.

#### LSP and architecture
It's applicable because there are users who depend on well-defined interfaces, and on the substitutability of the implementations of those interfaces.

#### Conclusion
* It can, and should, be extended to the level of architecture.
* A simple violation of substitutability, can cause a system’s architecture to be polluted with a significant amount of extra mechanisms.

### 10. ISP: The interface segregation principle
#### ISP and language
* Statically typed languages, force programmers to create declarations. It's these declarations in source code that create the source code dependencies that force recompilation and redeployment.
* In dynamically typed languages, such declarations don’t exist in source code. Instead, they are inferred at runtime. Thus there are no source code dependencies to force recompilation and redeployment.

This is the reason that dynamically typed languages create systems that are more flexible and less tightly coupled than statically typed languages.

#### ISP and architecture
It's harmful to depend on modules that contain more than you need.

#### Conclusion
* Depending on something that carries baggage that you don’t need can cause you troubles that you didn’t expect.

### 11. DIP: The dependency inversion principle
It tells us that the most flexible systems are those in which source code dependencies refer only to abstractions, not to concretions.

This means that the *use*, *import*, and *include* statements should refer only to source modules containing interfaces, abstract classes, or some other kind of abstract declaration.

Otherwise, there are some exceptions like the stable background of operating system and platform facilities because we know we can rely on them not to change.

#### Stable abstractions
Interfaces are less volatile than implementations.

* Don’t refer to volatile concrete classes.
    * It also puts severe constraints on the creation of objects and generally enforces the use of Abstract Factories.
* Don’t derive from volatile concrete classes.
* Don’t override concrete functions.
* Never mention the name of anything concrete and volatile.

#### Conclusion
* It's the most visible organizing principle in our architecture diagrams.

## IV. Component principles
* SOLID principles tell us how to arrange the bricks into walls and rooms.
* The component principles tell us how to arrange the rooms into buildings.

### 12. Components
* Components are the units of deployment.
    * They are the smallest entities that can be deployed as part of a system.

#### A brief history of components
* In the early years of software development, programmers controlled the memory location and layout of their programs.
* Devices were slow and memory was expensive.
    * This took a long time, and the larger your function library, the longer the compiler took. Compiling a large program could take hours.

#### Relocatability
* The compiler was changed to output binary code that could be relocated in memory by a smart loader.
* Now the programmer could tell the loader where to load the function library, and where to load the application.
    * They load only those functions that they needed.
* The loader could link the external references to the external definitions once it had determined where it had loaded those definitions.

#### Linkers
* It allowed programmers to divide their programs up onto separately compilable and loadable segments.
* It worked well with small programs and libraries.
* It was too slow to tolerate.
* Function libraries were stored on slow devices such a magnetic tape. Even the disks, back then, were quite slow.
* With *C* language, compiling each individual module was relatively fast, but compiling all the modules took a bit of time.
* Finally, disks started to shrink and got significantly faster.
    * Computer memory became cheap that much of the data on disk could be cached in RAM.
#### Conclusion
* These dynamically linked files, which can be plugged together at runtime, are the software components of our architectures.

### 13. Component cohesion
#### REP: The reuse/release equivalence principle
* The granule of reuse is the granule of release.
* Developers need to know when new releases are coming, and which changes those new releases will bring.
* It means that the classes and modules that are formed into a component must belong to a cohesive group.

#### CCP: The common closure principle
* Gather into components those classes that change for the same reasons and at the same times. Separate into different components those classes that change at different times and for different reasons.
* It says that a component should not have multiple reasons to change.
* If two classes are so tightly bound, either physically or conceptually, that they always change together, then they belong in the same component.
    * This minimizes the workload related to releasing, revalidating, and redeploying the software.

##### Similarity with SRP
* Gather together those things that change at the same times and for the same reasons.
* Separate those things that change at different times or for different reasons.

#### CRP: The common reuse principle
* Don’t force users of a component to depend on things they don’t need.
* It tells us more about which classes shouldn’t be together than about which classes should be together.
* It says that classes that are not tightly bound to each other should not be in the same component.

##### Relation to ISP
* Don’t depend on things you don’t need.

#### The tension diagram for component cohesion
* An architect who focuses on just the REP and CRP will find that too many components are impacted when simple changes are made.
* An architect who focuses too strongly on the CCP and REP will cause too many unneeded releases to be generated.

#### Conclusion
* The three principles of component cohesion describe a much more complex variety of cohesion.
* The balance is almost always dynamic.
    * The partitioning that is appropriate today might not be appropriate next year.

### 14. Component coupling
#### The acyclic dependecies principle
* Allow no cycles in the component dependency graph.
* The “morning after syndrome” occurs in development environments where many developers are modifying the same source files.

##### The weekly build
* It's common in medium-size projects.
* All the developers ignore each other for the first four days of the week.
    * They all work on private copies of the code, and don’t worry about integrating their work on a collective basis.
    * On Friday, they integrate all their changes and build the system.
* As the project grows, it becomes less feasible to finish integrating the project on Friday.
* Integration and testing become increasingly harder to do, and the team loses the benefit of rapid feedback.

##### Eliminating dependency cycles
* It splits the development environment into releasable components.
* When developers get a component working, they release it for use by the other developers.
* Each team can decide for itself when to adapt its own components to new releases of the components.
    * Moreover, integration happens in small increments.
* You must manage the dependency structure of the components.
    * If there are cycles in the dependency structure, then the “morning after syndrome” cannot be avoided.

##### The effect of a cycle in the component dependency graph
* Cycles make it very difficult to isolate components.
* Unit testing and releasing become very difficult and error prone.
* Cycles in the dependency graph, it can be very difficult to work out the order in which you must build the components.

##### Breaking the cycle
* Apply the Dependency Inversion Principle (DIP).
* Create a new component that parts that generate cycle depend on.

##### The jitters
* The second solution implies that the component structure is volatile in the presence of changing requirements.

#### Top-down design
* The component structure cannot be designed from the top down.
* It is not one of the first things about the system that is designed, but rather evolves as the system grows and changes.
* If we tried to design the component dependency structure before we designed any classes, we would likely fail rather badly.
    * We would not know much about common closure,
    * we would be unaware of any reusable elements, and
    * we would almost certainly create components that produced dependency cycles.

#### The stable dependencies principle
* Some of these components are designed to be volatile. We expect them to change.
* Any component that we expect to be volatile should not be depended on by a component that is difficult to change.
    * Otherwise, the volatile component will also be difficult to change.

##### Stability
* A component with lots of incoming dependencies is very stable because it requires a great deal of work to reconcile any changes with all the dependent components.

##### Stability metrics
* It counts the number of dependencies that enter and leave that component.
    * *Fan-in*: Incoming dependencies. This metric identifies the number of classes outside this component that depend on classes within the component.
    * *Fan-out*: Outgoing dependencies. This metric identifies the number of classes inside this component that depend on classes outside the component.
    * *I*: Instability: I = Fan-out / (Fan-in + Fan-out). This metric has the range [0, 1]. I = 0 indicates a maximally stable component. I = 1 indicates a maximally unstable component.

##### Not all components should be stable
* If all the components in a system were maximally stable, the system would be unchangeable.
    * This is not a desirable situation.
    * We want to design our component structure so that some components are unstable and some are stable.

##### Abstract components
* These abstract components are very stable and, therefore, are ideal targets for less stable components to depend on.

#### The stable abstractions principle
* A component should be as abstract as it is stable.

##### Where do we put the high-level policy
* If the high-level policies are placed into stable components, then the source code that represents those policies will be difficult to change.
* Open-closed principle tells us that it is possible and desirable to create classes that are flexible enough to be extended without requiring modification.
    * Abstract classes.

##### Introducing the stable abstractions principle
* It sets up a relationship between stability and abstractness.
* If a component is to be stable, it should consist of interfaces and abstract classes so that it can be extended.
    * Stable components that are extensible are flexible and do not overly constrain the architecture.

##### Measuring abstraction
* Its value is simply the ratio of interfaces and abstract classes in a component to the total number of classes in the component.
    * *Nc*: The number of classes in the component.
    * *Na*: The number of abstract classes and interfaces in the component.
    * *A*: Abstractness. A = Na ÷ Nc.
> 0: component **has no** abstract classes.</br>
> 1: component **only has** abstract classes.

##### The main sequence
* The components that are maximally stable and abstract at the upper left at (0, 1).
* The components that are maximally unstable and concrete are at the lower right at (1, 0).

##### The zone of pain (0, 0)
* This is a highly stable and concrete component.
* It is rigid.
    * It cannot be extended because it is not abstract, and it is very difficult to change because of its stability.
* Examples: database schema and concrete utility library.

##### The zone of uselessness (1, 1)
* This location is undesirable because it is maximally abstract, yet has no dependents.
* Such components are useless.
* Example: abstract classes that no one ever implemented.

##### Avoiding the zones of exclusion
* The most desirable position for a component is at one of the two endpoints of the Main Sequence.

##### Distance from the main sequence
* We can create a metric that measures how far away a component is from the main sequence:</br>
*D3*: Distance. `D = |A+I–1|`
    * A value of 0 indicates that the component is directly on the Main Sequence.
    * A value of 1 indicates that the component is as far away as possible from the main sequence.

#### Conclusion
* The dependency management metrics measure the conformance of a design to a pattern of dependency and abstraction that I think is a “good” pattern.
* A metric is not a god; it is merely a measurement against an arbitrary standard.

## V. Architecture

### 15. What is architecture?
* The architecture of a software system is the shape given to that system by those who build it.
    * The form of that shape is in the division of that system into components, the arrangement of those components, and the ways in which those components communicate with each other.
* The purpose of that shape is to facilitate the development, deployment, operation, and maintenance of the software system contained within it.
* The strategy behind that facilitation is to leave as many options open as possible, for as long as possible.
* There are many systems out there, with terrible architectures, that work just fine.
    * Their troubles do not lie in their operation; rather, they occur in their deployment, maintenance, and ongoing development.
* The primary purpose is to support the life cycle of the system.
    * Good architecture makes the system easy to understand, easy to develop, easy to maintain, and easy to deploy.
* The ultimate goal is to minimize the lifetime cost of the system and to maximize programmer productivity.

#### Development
* The architecture of a system should make that system easy to develop, for the team(s) who develop it.
* A small team of 5 developers can quite effectively work together to develop a monolithic system without well-defined components or interfaces.
* A system being developed by five different teams, each of which includes 7 developers, cannot make progress unless the system is divided into well-defined components with reliably stable interfaces.

#### Deployment
* A software system must be deployable.
* The higher the cost of deployment, the less useful the system is.
    * A goal of a software architecture, then, should be to make a system that can be easily deployed with a single action.

#### Operation
* The impact of architecture on system operation tends to be less dramatic than the impact of architecture on development, deployment, and maintenance.
* Almost any operational difficulty can be resolved by throwing more hardware at the system without drastically impacting the software architecture.
* A good software architecture communicates the operational needs of the system.

#### Maintenance
* It's a never-ending parade of new features and the inevitable trail of defects and corrections consume vast amounts of human resources.
* *Spelunking* is the cost of digging through the existing software, trying to determine the best place and the best strategy to add a new feature or to repair a defect.

#### Keeping options open
* All software systems can be decomposed into two major elements:
    * Policy embodies all the business rules and procedures. It's where the true value of the system lives.
    * Details are those things that are necessary to enable humans, other systems, and programmers to communicate with the policy, but that do not impact the behavior of the policy at all.
        * They include IO devices, databases, web systems, servers, frameworks, communication protocols, and so forth.
* The goal of the architect is to create a shape for the system that recognizes policy as the most essential element of the system while making the details irrelevant to that policy.
* A good architect maximizes the number of decisions not made.

#### Device independence
* The operating systems of the day abstracted the IO devices into software functions that handled unit records that looked like cards.

#### Junk mail
* Our programs had a shape.
* That shape disconnected policy from detail.
* The policy was the formatting of the name and address records.
* The detail was the device.
* We deferred the decision about which device we would use.

#### Physical addressing
* If you change the high-level policy of the system to be agnostic about the physical structure of the disk, it'll allow us to decouple the decision about disk drive structure from the application.

#### Conclusion
* Good architects carefully separate details from policy, and then decouple the policy from the details so thoroughly that the policy has no knowledge of the details and does not depend on the details in any way.
* Good architects design the policy so that decisions about the details can be delayed and deferred for as long as possible.

### 16. Independence
#### Use cases
* The architecture of the system must support the intent of the system.
* The architecture must support the use cases.
* The most important thing a good architecture can do to support behavior is to clarify and expose that behavior so that the intent of the system is visible at the architectural level.

#### Operation
* If the system must handle 100,000 customers per second, it must support that kind of throughput and response time for each use case that demands it.
* If the system must query big data cubes in milliseconds, then it must be structured to allow this kind of operation.

#### Development
* Any organization that designs a system will produce a design whose structure is a copy of the organization’s communication structure.
* A system that must be developed by an organization with many teams and many concerns must have an architecture that facilitates independent actions by those teams, so that the teams do not interfere with each other during development.

#### Deployment
* The goal is *immediate deployment*.
* It does not rely on dozens of little configuration scripts and property file tweaks.
* It does not require manual creation of directories or files that must be arranged just so.
* It helps the system to be immediately deployable after build.

#### Leaving options open
* It makes the system easy to change, in all the ways that it must change, by leaving options open.

#### Decoupling layers
* The system should be divided into decoupled horizontal layers—the UI, application-specific business rules, application-independent business rules, and the database.

#### Decoupling use cases
* Use cases are a very natural way to divide the system.
* If you decouple the elements of the system that change for different reasons, then you can continue to add new use cases without interfering with old ones.
* If you also group the UI and database in support of those use cases, so that each use case uses a different aspect of the UI and database, then adding new use cases will be unlikely to affect older ones.

#### Decoupling mode
* To run in separate servers, the separated components cannot depend on being together in the same address space of a processor.
* They must be independent services, which communicate over a network of some kind.

#### Independent develop-ability
* When components are strongly decoupled, the interference between teams is mitigated.
* So long as the layers and use cases are decoupled, the architecture of the system will support the organization of the teams, irrespective of whether they are organized as feature teams, component teams, layer teams, or some other variation.

#### Independent deployability
* Adding a new use case could be as simple as adding a few new jar files or services to the system while leaving the rest alone.

#### Duplication
* Duplication is generally a bad thing in software.
* True duplication, in which every change to one instance necessitates the same change to every duplicate of that instance.
* False or accidental duplication, if two apparently duplicated sections of code evolve along different paths if they change at different rates, and for different reasons then they are not true duplicates.
    * Return to them in a few years, and you’ll find that they are very different from each other.

#### Decoupling modes (again)
* Source level. We can control the dependencies between source code modules so that changes to one module do not force changes or recompilation of others (e.g., Ruby Gems).
* Deployment level. We can control the dependencies between deployable units such as jar files, DLLs, or shared libraries, so that changes to the source code in one module do not force others to be rebuilt and redeployed.
* Service level. We can reduce the dependencies down to the level of data structures, and communicate solely through network packets such that every execution unit is entirely independent of source and binary changes to others (e.g., services or micro-services).
* A good architecture will allow a system to be born as a monolith, deployed in a single file, but then to grow into a set of independently deployable units, and then all the way to independent services and/or micro-services. Later, as things change, it should allow for reversing that progression and sliding all the way back down into a monolith.
* A good architecture protects the majority of the source code from those changes.

#### Conclusion
* Decoupling modes should not be always a trivial configuration option.
* Decoupling mode of a system is one of those things that is likely to change with time, and a good architect foresees and appropriately facilitates those changes.

### 17. Boundaries: drawing lines
#### A couple of sad stories
* Drawing the boundary lines helped us delay and defer decisions, and it ultimately saved us an enormous amount of time and headaches.

#### Which lines do you draw, and when do you draw them?
* For instance, the database is a tool that the business rules can use indirectly.
* The business rules don’t need to know about the schema, or the query language, or any of the other details about the database.
* All the business rules need to know is that there is a set of functions that can be used to fetch or save data.
* This allows us to put the database behind an interface.
* The database could be implemented with Oracle, or MySQL, or Couch, or Datomic, or even flat files.
* The business rules don’t care at all.
* The database decision can be deferred and you can focus on getting the business rules written and tested before you have to make the database decision.

#### What about input and output?
* The IO is irrelevant.
* We often think about the behavior of the system in terms of the behavior of the IO.
* GUI could be replaced with any other kind of interface and the BusinessRules would not care.

#### Plugin architecture
* The history of software development technology is the story of how to create plugins to establish a scalable and maintainable system architecture.
* The core business rules are kept separate from those components that are either optional or that can be implemented in many different forms.

#### The plugin argument
* Boundaries are drawn where there is an axis of change.
* The components on one side of the boundary change at different rates, and for different reasons, than the components on the other side of the boundary.
    * GUIs change at different times and at different rates than business rules, so there should be a boundary between them.
    * Business rules change at different times and for different reasons than dependency injection frameworks, so there should be a boundary between them.

#### Conclusion
* To draw boundary lines in a software architecture, you first partition the system into components.
* This is an application of the *Dependency Inversion Principle* and the *Stable Abstractions Principle*.
    * Dependency arrows are arranged to point from lower-level details to higher-level abstractions.

### 18. Boundary anatomy
* The architecture of a system is defined by a set of software components and the boundaries that separate them.

#### Boundary crossing
* It's a function on one side of the boundary calling a function on the other side and passing along some data.

#### The dreaded monolith
* The fact that the boundaries are not visible during the deployment of a monolith does not mean that they are not present and meaningful.
* The simplest possible boundary crossing is a function call from a low-level client to a higher-level service.
* Communications between components in a monolith are very fast and inexpensive. They are typically just function calls. Consequently, communications across source-level decoupled boundaries can be very chatty.

#### Deployment components
* There may be a one-time hit for dynamic linking or runtime loading, but communications across these boundaries can still be very chatty.

#### Threads
* They're a way to organize the schedule and order of execution.
* They may be wholly contained within a component, or spread across many components.

#### Local processes
* It's the stronger physical architectural boundary.
* Local processes communicate with each other using sockets, or some other kind of operating system communications facility such as mailboxes or message queues.
* Communication across local process boundaries involve operating system calls, data marshaling and decoding, and interprocess context switches, which are moderately expensive.

#### Services
* It's the strongest boundary.
* It's a process, generally started from the command line or through an equivalent system call.
* Services do not depend on their physical location.
* Two communicating services may, or may not, operate in the same physical processor or multicore.
* They assume that all communications take place over the network.
* Communications across service boundaries are very slow compared to function calls.

#### Conclusion
* Most systems, other than monoliths, use more than one boundary strategy.
* The boundaries in a system are a mixture of local chatty boundaries and boundaries that are more concerned with latency.

### 19. Policy and level
* Software systems are statements of policy.
* A computer program is a detailed description of the policy by which inputs are transformed into outputs.

#### Level
* It's the distance from the inputs and outputs.
* The farther a policy is from both the inputs and the outputs of the system, the higher its level.
* The policies that manage input and output are the lowest-level policies in the system.

### 20. Business rules
* They are rules or procedures that make or save the business money.
* The *Critical Business Rules* are critical to the business itself, and would exist even if there were no system to automate them.
    * They usually require some data to work with.
* The critical rules and critical data are inextricably bound, so they are a good candidate for an object: *Entity*.

#### Entities
* It is an object within our computer system that embodies a small set of critical business rules operating on *Critical Business Data*.
* It contains the *Critical Business Data* or has very easy access to that data.
* The interface of the *Entity* consists of the functions that implement the *Critical Business Rules* that operate on that data.
* It stands alone as a representative of the business.
    * It is unsullied with concerns about databases, user interfaces, or third-party frameworks.
* It is pure business and nothing else.
* It is not a class.
    * You don’t need to use an object-oriented language to create an Entity.
    * It binds the *Critical Business Data* and the *Critical Business Rules* together in a single and separate software module.

#### Use cases
* Some business rules make or save money for the business by defining and constraining the way that an automated system operates.
* It is a description of the way that an automated system is used.
* It specifies the input to be provided by the user, the output to be returned to the user, and the processing steps involved in producing that output.
* It describes application-specific business rules as opposed to the *Critical Business Rules* within the *Entities*.
* It is impossible to tell whether the application is delivered on the web, or on a thick client, or on a console, or is a pure service.
* DIP: high-level concepts, such as *Entities*, know nothing of lower-level concepts, such as use cases. Instead, the lower-level use cases know about the higher-level *Entities*.
* Use cases depend on *Entities*; *Entities* do not depend on use cases.

#### Request and response models
* The use case class accepts simple request data structures for its input, and returns simple response data structures as its output. These data structures are not dependent on anything.
* They do not derive from standard framework interfaces such as HttpRequest and HttpResponse.
* *Entities* and request/response data structures have very differente purposes.
    * They change for very different reasons, so tying them together in any way violates the Common Closure and SRP.

#### Conclusion
* Business rules are the reason a software system exists.
    * They are the core functionality.
    * They carry the code that makes, or saves, money.
    * They are the family jewels.
* The business rules should remain pristine, unsullied by baser concerns such as the user interface or database used.
* The business rules should be the most independent and reusable code in the system.

### 21. Screaming architecture
#### The theme of an architecture
* Architectures are not (or should not be) about frameworks.
* Architectures should not be supplied by frameworks.
* Frameworks are tools to be used, not architectures to be conformed to.
* If your architecture is based on frameworks, then it cannot be based on your use cases.

#### The purpose of an architecture
* A good software architecture allows decisions about frameworks, databases, web servers, and other environmental issues and tools to be deferred and delayed.
    * Frameworks are options to be left open.
* A good architecture makes it easy to change your mind about those decisions, too.
* A good architecture emphasizes the use cases and decouples them from peripheral concerns.

#### But what about the web?
* The web is not an architecture. It's a delivery mechanism (IO device).
* Your system architecture should be as ignorant as possible about how it will be delivered.
* You should be able to deliver it as a console app, or a web app, or a thick client app, or even a web service app, without undue complication or change to the fundamental architecture.

#### Frameworks are tools, not ways of life
* Framework authors often believe very deeply in their frameworks.
    * They assume an all-encompassing, all-pervading, let-the-framework-do-everything position.
* Develop a strategy that prevents the framework from taking over that architecture.

#### Testable architectures
* You are be able to unit-test all those use cases without any of the complications of frameworks.

#### Conclusion
* Your architecture should tell readers about the system, not about the frameworks you used in your system.

### 22. The clean architecture
* For instance: hexagonal architecture, DCI (data context and iteration) and BCE.
* They all have the same objective, which is the separation of concerns.
* They all achieve this separation by dividing the software into layers.
    * At least, one layer for business rules, and another layer for user and system interfaces.
* They have following characteristics:
    * Independent of frameworks.
    * Testable.
    * Independent of the UI.
    * Independent of the database.
    * Independent of any external agency.

![clean architecture diagram](resources/clean_architecture_01.png "clean architecture diagram")

#### The dependency rule
* Source code dependencies must point only inward, toward higher-level policies.
* Nothing in an inner circle can know anything at all about something in an outer circle.
* Avoid anything of an outer circle that can impact in the inner circles.

##### Entities
* Entities encapsulate enterprise-wide Critical Business Rules.
* They are the business objects of the application.

##### Use cases
* They contains application-specific business rules.
* The orchestrate the flow of data to and from the entities, and direct those entities to use their Critical Business Rules to achieve the goals of the use case.
* This layer changes do not affect to the entities, database, framework neither UI.

##### Interface adapters
* It's a set of adapters that convert data from the format most convenient for the use cases and entities, to the format most convenient for some external agency such as the database or the web.
* This layer contains the MVC architecture of a GUI.
    * The models are likely just data structures that are passed from the controllers to the use cases, and then back from the use cases to the presenters and views.
* Data is converted from the form most convenient for entities and use cases, to the form most convenient for whatever persistence framework is being used
* No code inward of this circle should know anything at all about the database.
* Also in this layer is any other adapter necessary to convert data from some external form, such as an external service, to the internal form used by the use cases and entities.

##### Frameworks and drivers
* It is the outermost layer.
* You don’t write much code in this layer, other than glue code that communicates to the next circle inward.

##### Only four circles?
* Source code dependencies always point inward.
* As you move inward, the level of abstraction and policy increases.
* The outermost circle consists of low-level concrete details.
* As you move inward, the software grows more abstract and encapsulates higher-level policies.
* The innermost circle is the most general and highest level.

##### Crossing boundaries
* It begins in the controller, moves through the use case, and then winds up executing in the presenter.

##### Which data crosses the boundaries
* They're simple data structures. For instance: basic structs or DTOs.
* We don’t want to cheat and pass Entity objects or database rows.
* We don’t want the data structures to have any kind of dependency that violates the Dependency Rule.
* When we pass data across a boundary, it is always in the form that is most convenient for the inner circle.

#### Conclusion
* By separating the software into layers and conforming to the *Dependency Rule*, you will create a system that is intrinsically testable, with all the benefits that implies.
* When any of the external parts of the system become obsolete, such as the database, or the web framework, you can replace those obsolete elements with a minimum of fuss.

### 23. Presenters and humble objects
#### The humble object pattern
* It's a design pattern.
* It's a way to separate behaviors that are hard to test from behaviors that are easy to test.
* For instance -> GUI:
    * Presenter contains behaviour that it is easy to test.
    * View is the *Humble Object* because its behaviour is hard to test.

#### Presenters and views
* View only moves data into the GUI but does not process that data.
* The presenter is the testable object.
* The presenter's job is to accept data from the application and format it for the view.

#### Testing and architecture
* Testability is an attribute of good architectures.
* The separation of the behaviors into testable and non-testable parts often defines an architectural boundary.

#### Database gateways
* They exist between the use case interactors and the database.
* They are polymorphic interfaces that contain methods for:
    * create
    * read
    * update
    * delete
* We do not allow SQL in the use cases layer; instead, we use gateway interfaces that have appropriate methods.
* Those gateways are implemented by classes in the database layer.
* That implementation is the humble object.

#### Data mappers
* ORMs form another kind of Humble Object boundary between the gateway interfaces and the database.

#### Service listeners
* Applications must communicate with other services, or your application provides a set of services.
* We'll find the *Humble Object* pattern creating a service boundary.

#### Conclusion
* We should be able to find *Humble Objects* in our architectures to increase testability.

### 24. Partial boundaries
* Anticipatory design -> violation of YAGNI: "You Aren’t Going to Need It."
    * Architects, however, think, "Yeah, but I might."

#### Skip the last step
* Partial boundary is to do all the work necessary to create independently compilable and deployable components, and then simply keep them together in the same component.
    * It requires the same amount of code and preparatory design work as a full boundary.
    * It does not require the administration of multiple components.

#### One-dimensional boundaries
* The full-fledged architectural boundary uses reciprocal boundary interfaces to maintain isolation in both directions.
    * This is expensive both in initial setup and in ongoing maintenance.

#### Facades
* It's another boundary pattern.
* The boundary is simply defined by the Facade class.
* Lists all the services as methods.
* It deploys the service calls to classes that the client is not supposed to access.

#### Conclusion
* It is one of the functions of an architect to decide where an architectural boundary might one day exist, and whether to fully or partially implement that boundary.

### 25. Layers and boundaries
#### Hunt the wumpus
* The game rules will communicate with the UI component using a language-independent API.
* UI will translate the API into the appropriate human language.
* Any number of UI components can reuse the same game rules.
* The game rules do not know, nor do they care, which human language is being used.
* We’ll create an API that the game rules can use to communicate with the data storage component.

#### Clean architecture?
* You can divide the flow of data into two streams.
    * The first one is concerned with communicating with the user.
    * The second one is concerned with data persistence.
* Both streams meet at the top at GameRules, which is the ultimate processor of the data that goes through both streams.

#### Crossing the streams
* If the systems become more complex, the component structure may split into many such streams.

#### Splitting the streams
* In the systems become more complex, also, you can split the streams in their own microsersevice communicating via API.

#### Conclusion
* Architectural boundaries exist everywhere.
* Be aware that such boundaries, when fully implemented, are expensive.
* When such boundaries are ignored, they are very expensive to add in later, even in the presence of comprehensive test-suites and refactoring discipline.
* You must weigh the costs and determine where the architectural boundaries lie, and which should be fully implemented, and which should be partially implemented, and which should be ignored.

### 26. The main component
#### The ultimate detail
 * It is the initial entry point of the system.
 * Its job is to create all the Factories, Strategies, and other global facilities, and then hand control over to the high-level abstract portions of the system.
 * In that component, dependencies should be injected by a Dependency Injection framework.

#### Conclusion
* It sets up the initial conditions and configurations, gathers all the outside resources, and then hands control over to the high-level policy of the application.
* It is possible to have many Main components, one for each configuration of your application.