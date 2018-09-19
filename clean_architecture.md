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
