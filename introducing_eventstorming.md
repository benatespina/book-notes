# Introducing EventStorming
>An act of Deliberate Collective Learning 

By Alberto Brandolini.

### 1. What does EventStorming look like?
#### Challenging corporate processes
* Provide some big-picture level assessment of the state of the art in order to highlight the next critical actions.

#### The workshop
* A room without chairs neither tables.
* Stick 8-9 meters of a plotter paper roll onto the main wall.
* Dozen of black markers.
* Stockpile of sticky notes, mostly orange.
* Participants are a representation from the different company departments, plus the IT.

*We are asking you to write the key events in your domain as an orange sticky note, in a verb at past tense, and place them along a timeline.*

* This phase is around 2-3 hours, no more.
* After that, people can’t stop commenting on given business steps:
    * *and this is where everything screws up!*
    * *this never really works as expected.*
    * *it always takes ages to complete this step.*
* Capture every warning with a purple stickies with big exclamation marks.

#### Kicking off a startup
##### Day one
* Some orange stickies are duplicated: probably somebody had the same idea at the same moment, others look alike but with slightly different wordings.
* We keep them all, for the moment. We’ll choose the perfect wording later, once we’ll have some more information available.
* When a few stickies do not comply with the Domain Event definition, turn the sticky note 45° anticlockwise, to signal that something is wrong, without disrupting the discussion flow.
    * For example: instead of UserRegistered, Registration.
* We try to model processes before the detailed explanation from the domain expert.
* There is no description of the boring, easy-to-guess steps of the business process.
* We don’t talk about “the user needs to login in the system”, (boring) instead we talk about places where there’s more to learn.
* It’s more discovery than translation.
* It feels like developers are really getting into the problem.
* We start mentioning external systems that I start representing with larger pink stickies the external systems our cool new software will have to deal with.
    * External organizations
    * External services.
    * Online apps.
* When new terms arise, and the discussion shows that they have a very precise meaning in that Context, I start capturing key term definitions on a special sticky note and place them just below the normal flow.
* As a background process, I start to label specific areas of the business flow.
    * E.g. customer registration, claims or fraud detection.
    * We are starting to see independent models that could be implemented independently, with different, relatively well-defined purposes.

##### Day two
* Rewrite a few stickies with more precision and discarded some of the purple question marks.
* Introduce *Commands* representing user intentions/actions/decisions.
    * They are blue stickies.
    * E.g. Place Order or Send Invitation)
* Introduce *Actors* for specific user categories.
    * They are little yellow stickies.
* Commands are ultimately the result of some user decision, but thinking in terms of user decisions forces us to think in terms of the relevant data for a given decision step.
* *Read Models* are the representation of this data.
    * They are green stickies.
    * They are emerging as tools to support the decision making process happening in the user’s brain.
* *Policy* is the reactive logic that takes place after an event, and triggers commands somewhere else.
    * They are lilac.
* *Aggregate*.
    * They are traditional pale-yellow stickies.

#### Designing a new feature for a web app
* Where are domain events coming from?
    * maybe a Command triggered by a given User.
    * maybe the Domain Event has been triggered by some External System.
    * maybe it’s just the result of time passing, without any particular action involved.
    * maybe it’s just the consequence of some other event: whenever one thing happen, then another another one will happen.
        * It’s not always that obvious, so we set up a lilac sticky note for that.

#### Possible formats
* *Big Picture EventStorming*: the one to use to kick off a project, with every stakeholders involved.
* *Design Level EventStorming*: digs into possible implementation, often DDD-CQRS/ES style.
* *Value-Driven EventStorming*: A quick way to get into value-stream mapping, using storytelling like a possible platform.
* *UX-Driven EventStorming*: similar to the one above, focusing on the User/Customer Journey in the quest for usability and flawless execution.
* *EventStorming as a Retrospective*: using domain events to baseline a flow and expand the scope in order to look for improvement opportunities.
* *EventStorming as a Learning Tool*: perfect to maximize learning for new hires.

## A deep dive into problem space
### 2. A closer look to the problem space
#### Complex systems are non deterministic
* A computer software MUST be predictable, even in the few cases where the outcomes are fooling us.
    * When a deterministic system behaves in an unexpected way, it means that there is some hidden detail that we’re not considering.
    * Once we find out the missing piece, we might be able to explain and eventually reproduce the problem.

#### The role of expert
* In complex environments experts don’t have all the answers.
* They’ll have experience, analogies, ideas, and a network of peers to provide advices. But this won’t guarantee they’ll take the right decision.

#### Strategies for decision making
* In complex environments, the ability for the ones with the right information to take the right decision becomes crucial.
* Self organization is the hyped term describing the ability of a group of people to respond quickly and collaboratively to a problem.

#### Impediments to self organization
* People won’t self organize around a system they don’t understand.
* Without understanding of the system as a whole is very hard for people to self-organize.
* People won’t self organize around a system they can’t influence”

#### Organization silos
* They focus on their own area of responsibility (apparently a good idea).
* They progressively ignore what happens in other departments.
* They minimize the amount of learning needed for newcomers.
    * New hires don’t need to learn the whole thing in order to feel productive, just the portion of knowledge required to get their job done.
* Silos maximize overall ignorance within an organization.
* The really nasty trait if silos is their asymmetry: they’re easy to establish, and really hard to remove.

#### Business flows are crossing the silos
* The business flow crosses silos and expertise boundaries.
* One single source of knowledge won’t be enough to solve problems that are spanning across multiple areas.

#### Individual areas of expertise
* There are tree different areas:
    * Things I know really well.
    * Things I know but someone knows better.
    * Do not ask me about these things.

#### Hierarchy
* Hierarchies reinforce silos.
    * People get usually hired, trained and promoted inside a silo, and their duties and reward are normally within a silo boundary.
* Hierarchies partition responsibilities.
    * But an organization can be way more complex than just the sum of its departments.
* Problems within silo boundaries are addressed quicker than problems spanning multiple silos, or in the no man’s land.
    * In fact, problems which aren’t contained within a clear area of responsibility will tend to chronicize.

#### The clash between hierarchy and business flow
* There are three main networks connecting people.
    * The *hierarchy* as a top-down static structure, usually described by an organization-chart.
    * The *informal structure*, the affinity and competence-based network connecting the people who you’d ask for help, on the many different topics.
    * The *value creation network*, involving the people we have to collaborate with in order to deliver a service.
* Only the second and the third are creating value, the hierarchy is not.

#### The shape of the problem
* *Hard to solve* because it requires an agreement by many different stakeholders.
* *Hard to discuss*, because it involves many different actors, usually trying to escape blame by doing things right in their own silo.
* *Hard to visualize*, because it involves many interdependent aspects in different areas of expertise.

### 3. Pretending to solve the problem writing software
#### It's not about 'delivering software'
* Software development is a learning process, working code is a side effect.

#### The illusion of the underlying model
* Coding is actually the moment when ambiguities are discovered, in the form of a compile error, an unexpected behavior, or a bug.
* Conversations tolerate ambiguities, but Coding won’t forgive them.

#### Collecting nouns will lead you nowhere
* A recurring problem in enterprise software development is knowledge unreliability, and a strategy for modeling (nouns and data first) that seems perfect for being fooled.

#### The Product Owner fallacy
* Often, teams are kept in isolation because the PO is taking care of all the necessary conversations with different stakeholders.
* Product Owner becomes the only person who’s entitled of learning, while the development teams turns into a mere translation of requirements into code.
* Everything starts to smell like the old Analysts vs Developers segregation that was so popular in Waterfall-like environment.

#### The backlog fallacy
* It is optimized for delivery.
    * Having a cadence and releasing in small increments works great in order to provide a heartbeat and a measurable delivery progress.
* Habits are great in providing a healthy routine. But they create a lot of resistance when you need to change them, or to do something different.
* Repeatable weeks are optimized for planning and delivery, not for discovery.
    * In fact discovery is more likely to happen when we break our habits and do something distinctively different.

#### Embrace change
* Doing a thing twice costs more than doing it right at first shot.
* Iterative development doesn’t mean we shouldn’t try to start right.
* I do my best to start in the right way just because iterations are expensive and the fewer, the better.
    * Early discoveries :)
    * Early constraints :( 

#### What about emerging design?
* Emergent design is a great tool to help you find your way in scenarios of great uncertainty.
    * This is not the case when it comes to model business processes.
* Apply emergent design principles to a problem that has already a solution, it’s flushing money in the toilet.

#### Enter Domain-Driven Design
* Among all approaches to software development, Domain-Driven Design is the only one that focused on language as the key tool for a deep understanding of a given domain’s complexity.

#### The backlog fallacy (rewritten)
* What I do like about iterations
    * Frequent feedback: for a team needing to understand whether they’re on track or not, feedback is the highest value currency.
* What I don’t like about iterations
    * Iterations tend to repeat: team estimate, based on velocity. This is an implicit driver to make an iteration comparable to the previous one, maybe just to show that we improved a little.
    * Little space for doing things differently: there is a strong driver to turn frequent delivery into a habit. This is powerful, but suboptimal.
* Repeating things week after week is boring and boredom is the arch enemy of learning.

#### Model splitting is broken
* Splitting established model in a Database-centric architecture is among the most expensive refactorings in software development.
* Quite often the risks are so hard to evaluate that initiatives are killed before the start.
* Even teams embracing agile, these refactorings tend to float in the backlog in a loop of continuous procrastination, while bugs emerging from nobody knows where always get the top priority lane.