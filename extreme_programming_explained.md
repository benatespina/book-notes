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