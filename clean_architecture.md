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
    * This took a long time—and the larger your function library, the longer the compiler took. Compiling a large program could take hours.

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