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