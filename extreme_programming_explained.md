# Extreme Programming Explained
>Embrance Change, Second Edition

By Kent Beck.

### Chapter 1. What is XP?
* It's about social change.
* Don't protect yourself from success by holding back. Do your best and then deal with the consequences.
* It's a style of software development focusing on excellent application of programming techniques, clear communication, and teamwork.
* It's a path of improvement to excellence for people coming together to develop software. It is distinguished by:
    * Its short development cycles.
    * Its incremental planning approach.
    * Its ability to flexibly when the business needs change.
    * Its reliance on automated tests.
    * Its reliance on oral communication.
    * Its reliance on an evolutionary design process.
    * Its reliance on the close collaboration.
    * Its reliance on practices that work with both the short-term instincts of the team members and the long-term interests of the project.
* XP is a lightweight methodology for small-to-medium-sized teams developing software in the face of vague or rapidly changing requirements.
* In XP you only do what you need to do to create value for the customer.
* The values and principles behind XP are applicable at any scale.
* It's a methodology based on addressing constraints in software development.
* XP demands that participants learn a high level of technique in service of the team's goals.
* The risks in the development process:
    * Schedule slips.
        * It calls for short release cycles, so the scope of any slip is limited. Within a release, XP uses one-week iterations of customer-requested features to create fine-grained feedback about progress. XP calls for implementing the highest priority features first, so any features that slip past the release will be of lower value.
    * Project canceled.
        * It asks the business-oriented part of the team to choose the smallest release that makes the most business sense, so there is less to go wrong before deploying and the value of the software is greatest.
    * System goes sour.
        * It creates and maintains a comprehensive suite of automated tests, which are run and rerun after every change to ensure a quality baseline. XP always keeps the system in deployable condition. Problems are not allowed to accumulate.
    * Defect rate.
        * It tests from the perspective of both programmers writing tests function-by-function and customers writing tests program-feature-by-program-feature.
    * Business misunderstood.
        * The specification of the project is continuously refined during development, so learning by the customer and the team can be reflected in the software.
    * Business changes.
        * It shortens the release cycle, so there is less change during the development of a single release. During a release, the customer is welcome to substitute new functionality for functionality not yet completed. The team doesn't even notice if it is working on newly discovered functionality or features defined years ago.
    * False feature rich.
        * XP insists that only the highest priority tasks are addressed.
    * Staff turnover.
        * XP asks programmers to accept responsibility for estimating and completing their own work, gives them feedback about the actual time taken so their estimates can improve, and respects those estimates. XP also encourages human contact among the team, reducing the loneliness that is often at the heart of job dissatisfaction. New team members are encouraged to gradually accept more and more responsibility.

## Part 1. Exploring XP

### Chapter 2. Learning to Drive
* XP paradigm: Stay aware. Adapt. Change.
* The problem isn't the change, because change is going to happen; the problem, rather, is our inability to cope with change.
* XP lets you adapt by making frequent, small corrections; moving towards your goal with deployed software at short intervals.
    * You don't wait a long time to find out if you were going the wrong way.

### Chapter 3. Values, principles and practices
* Values are the roots of the things we like and don't like in a situation.
* Values bring purpose to practices.
* Values are universal.
* Practices are evidence of values.
* Practices bring accountability to values.
* Practices are intensely situated.
* Principles are domain-specific guidelines for life.
* First you have to garden, then join the community of gardeners, then teach others to garden. Then you are a gardener.

### Chapter 4. Values
* What actually matters is not how any given person behaves as much as how the individuals behave as part of a team and as part of an organization.

#### Communication
* When you encounter a problem, ask yourselves if the problem was caused by a lack of communication.
* What communication do you need now to address the problem?
* What communication do you need to keep yourself out of this trouble in the future?

#### Simplicity
* When you need to change to regain simplicity, you must find a way from where you are to where you want to be.
* Improving communication helps achieve simplicity by eliminating unneeded or deferrable requirements from today's concerns.
* Achieving simplicity gives you that much less to communicate about.

#### Feedback
* Change is inevitable, but change creates the need for feedback.
* Feedback comes in many forms:
    * Opinions about an idea, yours or your teammates'.
    * How the code looks when you implement the idea.
    * Whether the tests were easy to write.
    * Whether the tests run.
    * How the idea works once it has been deployed.
* XP teams strive to generate as much feedback as they can handle as quickly as possible.

#### Courage
* Doing something without regard for the consequences is not effective teamwork.
* The courage to speak truths, pleasant or unpleasant, fosters communication and trust. The courage to discard failing solutions and seek new ones encourages simplicity.
* The courage to seek real, concrete answers creates feedback.

#### Respect
* If members of a team don't care about each other and what they are doing, XP won't work.
* If members of a team don't care about a project, nothing can save it.

#### Others
* Other important values include safety, security, predictability, and quality-of-life.

### Chapter 5. Principles
#### Humanity
* Basic safety: freedom from hunger, physical harm, and threats to loved ones. Fear of job loss threatens this need.
* Accomplishment: the opportunity and ability to contribute to their society.
* Belonging: the ability to identify with a group from which they receive validation and accountability and contribute to its shared goals.
* Growth: the opportunity to expand their skills and perspective.
* Intimacy: the ability to understand and be understood deeply by others.

#### Economics
* Time value of money: a dollar today is worth more than a dollar tomorrow.
* Software development is more valuable when it earns money sooner and spends money later.
* If I can redeploy my media scheduling program for a variety of scheduling-related tasks, it is much more valuable than if it can only be used for its originally intended purpose.

#### Mutual benefit
* It's about searching for practices that benefit me now, me later, and my customer as well.
* Extensive internal documentation of software is an example of a practice that violates mutual benefit. I am supposed to slow down my development considerably so some unknown person in a potential future will have an easier time maintaining this code. I can see a possible benefit to the future person should the documentation still happen to be valid, but no benefit now.
* XP solves like this:
    * I write automated tests that help me design and implement better today. I leave these tests for future programmers to use as well.
    * I carefully refactor to remove accidental complexity, giving me both satisfaction and fewer defects and making the code easier to understand for those who encounter it later.
    * I choose names from a coherent and explicit set of metaphors which speeds my development and makes the code clearer to new programmers.

#### Self-similarity
* Just because you copy a structure that works in one context doesn't mean it will work in another.
* Having the system-level tests before you begin implementation simplifies design, reduces stress, and improves feedback.

#### Improvement
* To do the best you can today, striving for the awareness and understanding necessary to do better tomorrow.
* It doesn't mean waiting for perfection in order to begin.
* The history of software development technology shows us gradually eliminating wasted effort.

#### Diversity
* Teams where everyone is alike, while comfortable, are not effective. 
* Teams need to bring together a variety of skills, attitudes, and perspectives to see problems and pitfalls, to think of multiple ways to solve problems, and to implement the solutions.
* Two ideas about a design present an opportunity, not a problem.

#### Reflection
* Good teams don't just do their work, they think about how they are working and why they are working.
* They don't try to hide their mistakes, but expose them and learn from them.
* Reflection moments:
    * Official: do pair programming and continuous integration.
    * Non-official: conversation with a spouse or friend, vacation, non-software-related reading and activities, shared meals and coffee breaks.
* Reflection comes after action.
* Learning is action reflected.
* To maximize feedback, reflection in XP teams is mixed with doing.

#### Flow
* It's delivering a steady flow of valuable software by engaging in all the activities of development simultaneously.
* It suggests that for improvement, deploy smaller increments of value ever more frequently.

#### Opportunity
* To reach excellence, problems need to turn into opportunities for learning and improvement, not just survival.
* Turning problems into opportunities takes place across the development process.
* It maximizes strengths and minimizes weaknesses.

#### Redundancy
* Defects corrode trust and trust is the great waste eliminator.
* Defects are a critical, difficult problem.
* Defects are addressed in XP by many of the practices:
    * pair programming
    * continuous integration
    * sitting together
    * real customer involvement
    * daily deployment

#### Failure
* Failure is not a waste if it imparts knowledge.
* Knowledge is valuable and sometimes hard to come by.
* Failure may not be avoidable waste.

#### Quality
* Projects don't go faster by accepting lower quality.
* Pushing quality higher often results in faster delivery.
* Quality isn't a purely economic factor. People need to do work they are proud of.
* A concern for quality is no excuse for inaction.
    * If you don't know a clean way to do a job that has to be done, do it the best way you can.
    * If you know a clean way but it would take too long, do the job as well as you have time for now. Resolve to finish doing it the clean way later.
    * You have to live with two architectures solving the same problem while you transition from one to the other.
        * Then the transition itself becomes a demonstration of quality: making a big change efficiently in small, safe steps.

#### Baby steps
* It's always tempting to make big changes in big steps.
* Baby steps acknowledge that the overhead of small steps is much less than when a team wastefully recoils from aborted big changes.

#### Accepted responsibility
* Responsibility cannot be assigned, it can only be accepted.
* If someone tries to give you responsibility, only you can decide if you are responsible or if you aren't.
* With responsibility comes authority.
* When a process expert can tell me how to work, but doesn't share in that work or its consequences, authority and responsibility are misaligned.

#### Conclusion
* Use principles to understand the practices better and to improvise complementary practices when you don't find one that suits your purpose.
* The principles give you a better idea of what the practice is intended to accomplish.

### Chapter 6. Practices
* They are kind of things you'll see XP teams doing day-to-day.
* If the situation changes, you choose different practices to meet those conditions.
    * Your values do not have to change in order to adapt to a new situation.
    * Some new principles may be called when you change domains.
* They are a vector from where you are to where you can be with XP.

### Chapter 7. Primary practices
#### Sit together
* No matter what the client says the problem is, it is always a people problem. Technical fixes alone are not enough.
* How important it is to sit together, to communicate with all our senses.
* Tearing down the cubicle walls before the team is ready is counterproductive.
* A team that knows that physical proximity enhances communication and that has learned the value of communication will open up their own space, given the chance.

#### Whole team
* Include on the team people with all the skills and perspectives necessary for the project to succeed. - Cross-functional teams.
* People need a sense of team:
    * We belong.
    * We are in this together.
    * We support each others' work, growth, and learning.
* What constitutes a “whole team” is dynamic.
* If a set of skills or attitudes becomes important, bring a person with these skills on the team.
* If someone is no longer necessary, he can go elsewhere.

#### Informative workspace
* Make your workspace about your work.
    * Walk into the team space and get a general idea of how the project is going in 15 seconds.

#### Energized work
* Work only as many hours as you can be productive and only as many hours as you can sustain.
* It's easy to remove value from a software project; but when you're tired, it's hard to recognize that you're removing value.
* When you're sick, respect yourself and the rest of your team by resting and getting well. Taking care of yourself is the quickest way back to energized work.
* You can make incremental improvements in work hours.
    * Declare a two-hour stretch each day as Code Time. Turn off the phones and email notification, and just program for two hours.

#### Pair programming
* It's a dialog between two people simultaneously programming (and analyzing and designing and testing) and trying to program better.
    * Keep each other on task.
    * Brainstorm refinements to the system.
    * Clarify ideas.
    * Take initiative when their partner is stuck, thus lowering frustration.
    * Hold each other accountable to the team's practices.
* Pair programming is tiring but satisfying.
* Rotate pairs can be a good idea, switching at natural breaks in development.

##### Pairing and personal space
* Different individuals and cultures are comfortable with different amounts of body space.
* Personal hygiene and health are important issues when pairing.
* Ideally, emotions at work will be about work.
* It is important to respect individual differences when pairing.
* If you aren't comfortable, the team isn't doing as well as it could.

#### Stories
* As soon as a story is written, try to estimate the development effort necessary to implement it.
* Stories are estimated very early in their life.
* Get the greatest return from the smallest investment.
* You can't make a good decision based on image alone.
    * You need to know your constraints, both cost and intended use.

#### Weekly cycle
* The nice thing about a week as opposed to two or three is that everyone is focused on Friday.
* The team's job—programmers, testers, and customers together—is to write the tests and then get them to run in five days.
* If you get to Wednesday and it is clear that all the tests won't be running, that the stories won't be completed and ready to deploy, you still have time to choose the most valuable stories and complete them.
* Planning is a form of necessary waste.

#### Quarterly cycle
* Using a quarter as a planning horizon synchronizes nicely with other business activities that occur quarterly.
* They're comfortable for interaction with external suppliers and customers.
* They're good for team reflection, finding gnawing-but-unconscious bottlenecks.

#### Slack
* You may have to begin slack with yourself, telling yourself how long you actually think a task will take and giving yourself time to do it, even if the rest of the organization is not ready for honest and clear communication.

#### Ten-minute build
* Automatically build the whole system and run all of the tests in ten minutes.
    * A build that takes longer than ten minutes will be used much less often, missing the opportunity for feedback. 

#### Continuous integration
* Team programming is a divide, conquer, and integrate problem.
* The integration step is unpredictable, but can easily take more time than the original programming.
    * The longer you wait to integrate, the more it costs and the more unpredictable the cost becomes.
* Continuous integration should be complete enough that the eventual first deployment of the system is no big deal.

#### Test-first programming
* It addresses many problems.
    * Scope creep: if you really want to put that other code in, write another test after you've made this one work.
    * Coupling and cohesion: if it's hard to write a test, it's a signal that you have a design problem, not a testing problem. Loosely coupled, highly cohesive code is easy to test.
    * Trust: writing clean code that works and demonstrating your intentions with automated tests, you give your teammates a reason to trust you.
    * Rhythm: it's clearer what to do next: either write another test or make the broken test work. Code, refactor, test, code, refactor.
* Continuous testing reduces the time to fix errors by reducing the time to discover them.

#### Incremental design
* It says that design done close to when it is used is more efficient.
* Refactoring is a discipline of design that codifies these recurring patterns of changes.
    * They can occur at any level of scale.
    * Few design decisions are difficult to change once made.
* The result is systems that can start small and grow as needed without exorbitant cost.

#### And now...
* They provide a foundation of respect, communication, and feedback that fosters simplicity and courage.
* The team members can use their increasing confidence and competence to build relationships inside and outside the team.

### Chapter 8. Getting started
* One option is to use XP-style planning.
    * Write stories about improving your software development process. 
        * Automate the build, Test first all day, Pair program with Joe for two hours.
    * Estimate how long each will take.
    * Figure out your budget for process improvement.
    * Pick a story to work on first.
    * Adapt as you discover what is easy or valuable and what is difficult.
* Change always starts at home.
    * Programmers can start writing tests first.
    * Testers can automate their tests.
    * Customers can write stories and set clear priorities.
    * Executives can expect transparency.

#### Mapping the practices
* An exercise for discovering what each practice means for you and your team.
    * In the middle is the practice.
    * Directly below that is the purpose of the practice as I see it: to keep my work and my life in balance.
    * Attached to the practice are factors that affect it and, in this case, symptoms that the practice isn't going well.
    * Map whatever issues come to mind when you think about a practice.

#### Conclusion
* SW development is capable of much more than it is currently delivering.
* Defects should be notable because they are rare.
* Major scope adjustments because of lack of progress should only need to occur in the first half of schedules.
* Initial deployment of software should come after a small percentage of the project budget is spent.
* Teams should be able to grow and shrink without catastrophic consequences.

### Chapter 9. Corollary practices
#### Real customer involvement
* The closer customer needs and development capabilities are, the more valuable development becomes.
* When you act trustworthy and have nothing to hide, you are more productive.
* When you are ready with accurate estimates and low defect rates, including customers in the development process fosters trust and encourages continued improvement.

#### Incremental deployment
* Big deployments have a high risk and high human and economic costs.
* Find a little piece of functionality or a limited data set you can handle right away. Deploy it.
* You'll have to find a way to run both programs in parallel, splitting and merging files or training some users to use both programs.
    * This scaffolding, technical or social, is the price you pay for insurance.

#### Team continuity
* Value in software is created not just by what people know and do but also by their relationships and what they accomplish together.
* Ignoring the value of relationships and trust just to simplify the scheduling problem is false economy.

#### Shrinking teams
* Toyota Production System:
    * As a team grows in capability, keep its workload constant but gradually reduce its size.
    * This frees people to form more teams.
    * When the team has too few members, merge it with another too-small team.
* Figure out how many stories the customer needs each week.
* Strive to improve development until some of the team members are idle.
* Then you're ready to shrink the team and continue.

#### Root-cause analysis
* Write an automated system-level test that demonstrates the defect, including the desired behavior. This can be done by the customer, by customer support, or by developers.
* Write a unit test with the smallest possible scope that also reproduces the defect.
* Fix the system so the unit test works. This should cause the system test to pass also. If not, return to the previous point.
* Once the defect is resolved, figure out why the defect was created and wasn't caught. Initiate the necessary changes to prevent this kind of defect in the future.

#### Shared code
* If something is wrong with the system and fixing it is not out of scope for what I'm doing right now, I should go ahead and fix it.
* Until the team has developed a sense of collective responsibility, no one is responsible and quality will deteriorate.
* People will make changes without regard for the team-wide consequences.
* Some techniques like pair programming and continuous integration are interesting to try to avoid selfish conducts.

#### Code and tests
* Maintain only the code and the tests as permanent artifacts.
* Generate other documents from the code and tests.
* Rely on social mechanisms to keep alive important history of the project.
* The valuable decisions in software development are:
    * What are we going to do?
    * What aren't we going to do?
    * How are we going to do what we do?

#### Single code base
* Multiple code streams are an enormous source of waste in software development.
* Don't make more versions of your source code.
* Rather than add more code bases, fix the underlying design problem that is preventing you from running from a single code base.

#### Daily deployment
* Put new software into production every night.
* Any gap between what is on a programmer's desk and what is in production is a risk.
* A programmer out of sync with the deployed software risks making decisions without getting accurate feedback about those decisions.
* There are technical, psychological or social and business-related barriers.
    * Working to remove it and then letting more frequent deployment come as a natural consequence will help you improve development.

#### Negotiate scope contract
* Reduce risk by signing a sequence of short contracts instead of one long one.
* They're a mechanism for aligning the interests of suppliers and customers to encourage communication and feedback.
* They give everyone the courage to do what looks right today, not do something ineffective just because it is in the contract.

#### Pay-per-use
* Money is the ultimate feedback.
* Connecting money flow directly to software development provides accurate, timely information with which to drive improvement.
* If you can't implement pay-per-use, you might be able to go to a subscription model.

#### Conclusion
* The primary and corollary practices are the core of excellence for software development teams.

### Chapter 10. The whole XP team
#### Testers
* Much of the responsibility for catching trivial mistakes is accepted by the programmers.
    * Test-first programming results in a suite of tests that help keep the project stable.
* The role of testers is to help defining and specifying what will constitute acceptable functioning of the system before the functionality has been implemented.

#### Interaction designers
* They choose overall metaphors for the system, write stories, and evaluate usage of the deployed system to find opportunities for new stories.

#### Architects
* THey look for and execute large-scale refactorings, write system-level tests that stress the architecture, and implement stories.
* Making big architectural changes in small, safe steps is one of the challenges for an XP team.
* They help choose the most appropriate fracture lines and then follow the system as a whole.
    * They keep the big picture in mind as the groups focus on their smaller section.

#### Project managers
* They facilitate communication inside the team and coordinate communication with customers, suppliers, and the rest of the organization.
* They are responsible for keeping plans synchronized with reality.
* They facilitate communication within the team, increasing cohesiveness and confidence.

#### Product managers
* They help the team decide priorities by analyzing the differences between actual and assumed requirements.
    * They adapt the story and theme to what is really happening now.
* Stories should be sequenced for business, not technical, reasons.
* The goal is a working system from the first week.
* They encourage communication between customers and programmers, making sure the most important customer concerns are heard and acted on by the team.

#### Executives
* They provide an XP team with courage, confidence, and accountability.
* They articulate and maintain large-scale goals.
* They have a right to see not just good software coming from the team, but continuing improvement as well.
* They are free to ask for explanations about any aspect that makes sense.
* Two metrics to measure the health of XP teams:
    * Number of defects found after development. Each is an opportunity for the team to learn and improve.
    * The time lag between the beginning of investment in an idea and when the idea first generates revenue.

#### Technical writers
* They provide early feedback about features and to create closer relationships with users.
* They create closer relationships with users.
    * They help them learn about the product, listening to their feedback, and addressing confusion with further publications or new stories.

#### Users
* They help write and pick stories and make domain decisions during development.

#### Programmers
* They estimate stories and tasks, break stories into tasks, write tests, write code to implement features, automate tedious development process, and gradually improve the design of the system.
* They need to develop good social and relationship skills.

#### Human resources
* Two challenges:
    * Reviews.
    * Hiring.
* Valuable employees in XP:
    * Act respecful.
    * Play well with others.
    * Take initiative.
    * Deliver on their commitments.
* XP teams put much more emphasis on teamwork and social skills.
* The best interviewing technique is to have the candidate work with the team for a day.
    * Pair programming provides an excellent test of technical and social skills. 

#### Roles
* The goal is to have everyone contribute the best he has to offer to the team's success.
* As the team matures, keep in mind the alignment of authority and responsibility.
* Everyone on the team can recommend changes, but they should be prepared to back up their concerns with action.

### Chapter 11. The theory of Constraints
* In any system there is one constraint at a time.
* To improve overall system throughput you have to:
    * Find the constraint.
    * Make sure it is working full speed.
    * Find ways of either increasing the capacity of the constraint, offloading some of the work onto non-constraints, or eliminating the constraint entirely.
* The Theory of Constraints says there is always a constraint.
* When we eliminate one constraint we create another.
* Micro-optimization is never enough.
* To improve our results we must look at the whole situation before deciding what to change.
* Software development is a human process not a factory.

### Chapter 12. Planning: managing scope
* It starts with putting the current goals, assumptions, and facts on the table.
    * With current, explicit information, you can work toward agreement about what's in scope, what's out of scope, and what to do next.
* Planning is complicated because the estimates of the cost and value of stories are uncertain.
* Lowering the quality can create the illusion of progress this way, but you pay in reduced satisfaction and damaged relationships.
* There is a limit to how much work can be done in a day.
    * More time at the desk does not equal increased productivity for creative work.
* It's open and accessible, that reflects the kind of relationships that make for the most valuable software development.