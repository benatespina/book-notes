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

### 4. Running a big picture workshop
* It is one single large scale workshop that involves all the key people that we expect cooperate in order to solve critical business problems.
* In this workshop:
    * we'll build a behavioral model of an entire line of business, highlighting collaborations, boundaries, responsibilities and the different perspectives of the involved actors and stakeholders.
    * we'll discover and validate the most compelling problem to solve.
    * we'll highlight the major risks involved with the status quo, and possible new solutions.

#### Key ingredients for a Big Picture EventStorming
* Invite the right people.
* A suitable location with unlimited modelling space.
* One facilitator.

#### Invite the right people
* A good mix of knowledge and curiosity, but most of all you’ll need people that actually care about the problem.

#### Room setup
* It has a long straight wall where our paper roll can be placed as our modeling surface. 8 meters is the minimum. The more, the better.
* There’s enough space for people to move by the modeling surface. People will need to see the forest and the trees.
* Seats are not easily available. They’ll be needed after a couple of hours, but they’re just terrible at the beginning. Stacking them in a corner is usually a good enough signal.
* The paper roll is put in place on the long straight wall.
* There is a flip-chart or an equivalent tool to be used as a legend, for the workshop.
* There is plenty of sticky notes and markers for everyone.
* There’s enough healthy food and beverages to make sure that nobody will be starving.
* There is a timer: some phases will need to be time-boxed. A visible clock might come in handy.

#### Phase: kick-off
* Start the workshop with a quick informal presentation round, in order to discover everyone’s background, attitude and goals, and to allow everyone to introduce themselves.
* "We are going to explore the business process as a whole31 by placing all the relevant events along a timeline. We’ll highlight ideas, risks and opportunities along the way."
* Take care of the people’s feelings. It’s not machinery, it’s people. You won’t regret it.

#### Phase: Chaotic Exploration
* We will name orange sticky notes Domain Events and place them along a timeline in order to represent our whole business flow.
    * It has to be an orange sticky note.
    * It needs to be phrased at past tense.
    * It has to be relevant for the domain experts.
* We will build our narrative as a sequence of related events.
* Make the notation explicit from the beginning in a visible legend.

#### Getting into a state of flow
* It is the hardest moment for the facilitators, people will look at them for guidance, but their job is to support, not to actively lead.
* An icebreaker, the person that places the first sticky note somewhere in the middle of your modeling surface, is your best ally.
* Once the ice is broken, the workshop ignites.
    * It turns into a chaotic, massively parallel activity where everybody is contributing to the model.

#### Rephrasing to use past tense
* Confidence and engagement are more important than precision or compliance to an arbitrary standard, at this stage.
* Some people are tempted not to dig deeper into those phases, but we do care about the details.
    * We provided an unlimited modeling surface exactly to be able to see these processes at the right level of detail.
    * The right level is Domain Events.

#### Growing the timeline organically
* During the chaotic phase, there’s no common denominator on people’s behavior.
    * Some might form small committees trying to agree on a common phrasing.
        * Facilitator should politely step in and break the ring, since discussing to reach an agreement on every single sticky note, before writing it would kill workshop throughput and hide exactly the contradictions we want to explore.
    * Some might work mostly alone, dropping the bulk of their personal expertise in a single strip of orange sticky notes, ignoring the surrounding world.
    * Some might have no idea about what to write, and they’ll need to be reassured that guessing is a totally legitimate action in EventStorming.
* I am not expecting many conversations at this stage.
    * After breaking the committee circles, people will eventually start working on their own.
* I expect to have locally ordered clusters in a disordered whole, and the timeline constraint to be broken in a few places.

#### Phase: Enforcing the timeline
* Experts may just take a marker and write down everything they know about the domain, and place everything on the wall in a single batch.
* The goal is to make sure we are actually following the timeline: we’d like the flow of events to be consistent from the beginning to the end.

#### Provide Structure
* The main impediment to efficient sorting is the availability of empty space.
    * This is a good moment to add more space.
* If you can't, choose a shorting strategy.
    * Pivotal Events
    * Swimlanes
    * Temporal Milestones
    * Chapter Sorting
    * The Usual Suspects

#### Pivotal events
* With Pivotal Events we start looking for the few most significant events in the flow.
* My favorite tool here is a colored label tape, that I place below the pivotal event sticky note, as a boundary between the different phases of the business flow.
* Usually 4-5 key events are enough to allow quicker sorting of the remaining events.

#### Swimlanes
* Separate the whole flow into horizontal swimlanes, assigned to given actors or departments.
* It plays very well for a single process, or for a few distinct processes that tend to happen in parallel to the main business flow.

#### Temporal milestones
* It is the best choice when there are many concurrent processes or too much misalignment about what comes first.
* Make the temporal frames roughly visible on the surface so that everybody can place their items more or less where they belong.
* Place blue sticky notes on top of the modeling surface with some typical time indicator like *1 year before*, *6 month before* or *1 month before*.

#### Chapter sorting
* We run a quick extraction round of the key Chapters of the whole business story. This will usually lead to 15-25 chapters (usually I write them on larger yellow stickies).
* We quickly sort the chapters on a different surface (often a window), resulting in a skeleton of the desired structure of the flow.
* We then apply the chapters structure on the main flow and remodel everything accordingly.

#### Combining multiple strategies
* A single strategy is not enough to rule the complexity of the whole thing, and that you’ll have to combine different approaches.

#### Highlight inconsistencies with Hot Spots
* Enforcing global consistency won’t happen without frictions.
* The facilitator should look for places where the discussion is getting hot and marking them with a purple sticky note (Hot Spots).
* HotSpots capture comments and remarks about issues in the narrative, and I am expecting to find quite a few of them.

#### People and Systems
* Little ellow stickies for people.
* Large pink stickies for external systems.

#### Fuzziness in action
* They allow every participant to be part of the discussion, without a specific background.
* Adding significant people adds more clarity, but the goal is to trigger some insightful conversation: where the behavior depends on a different type of user, where special actions need to be taken, and so on.

#### External systems
* It is a piece of the whole flow which is outside of our control.

#### Phase: Explicit walk-through
* To enforce consistency during this phase is to ask someone to walk through the sequence of events, while telling the story that connects them.
* It is a good idea to change the narrator, in a relay race fashion, once we reach pivotal events “and this is where Mary’s team takes over” offering the possibility to see the experts in action in their own territory.
* Facilitator should make sure that the spoken storytelling is aligned with the model: missing events or systems can be added on the fly.

#### Manage discussions
* Some discussions cannot be solved during the workshop.
* When conversation is getting non-conclusive, I mark it with a Hot Spot and move on.
* Some other discussions are interesting for everybody, and the workshop might be the one chance in a lifetime to get a long awaited clarification.

#### Reverse narrative
* Pick an event from the end of the flow, then look for the events that made it possible.
    * The event must be consistent: it has to be the direct consequence of previous events with no magic gaps in between.
* If some step or piece of information is missing, we’ll need to add the corresponding event or sub-flow to our model.
* Every other event needs to be consistent as well, so you may want to repeat it ad libitum.

#### Picking candidate events
* Terminal events (the ones at the end of the flow that seem to "settle everything").
* Pivotal events (the ones closing a specific phases) that often cover a checklist-like structure.

#### Looking for key impediments
* Offer a 10-15 minutes time-box to add *Problems* and *Opportunities* to our model.
* Problems are represented with our familiar Hotspot purple stickies.
* Opportunities with an optimistic green stickies.

#### Arrow voting
* Every workshop participants has the possibility to cast two votes.
    * Votes are little blue stickies with an arrow.
* Arrow should point towards a purple problem or a green opportunity. 
    * Everybody should be free to choose their voting criteria.
    * Votes could be pointing to the same target or to two independent targets.
* Voting happens more or less simultaneously, no voting catwalk.

#### Should we always be voting?
* Sometimes it should not be called:
    * Wrong people mix in the room.
        * A single partisan perspective could be a trusted source of an opinion, but not a system wide diagnosis.
    * Wrong scope.
        * Exploring the whole was beyond our reach. We might vote, but the real key constraint might be hidden somewhere else.
    * Non-disclosure constraints.
        * In a pre-sales requirements exploration scenario, an organization might be open to exploration, but not to explore their vulnerabilities.
    * Just too early.
        * A startup in inception phase doesn't have a real impediments yet, just a list of assumptions to be challenged and questions to be answered.

### 5. Playing with value - part 1
(It is not written yet).