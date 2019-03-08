---
layout: post
title: "Notes from Growing Object-oriented Software Guided by Tests"
date: 2019-02-21 11:00:00 
comments: true
description: "Notes from Notes from Growing Object-oriented Software Guided by Tests"
categories:
- books
- object oriented
- tests
- test driven development
permalink: 2019-02-21-growing-object-oriented

---

<p align="center">
    <img src="{{ site.url}}/assets/images/books/growing.jpg" alt="building microservices" width="200"/>
</p>

## 1. Intro

* mock objects are substitute implementations for testing how an object interacts with its neighbors.
* testing is no long just about keeping defects from the users; instead, it's about helping the team to understand the features that the users
need and to deliver those features reliably and predictably.
* *"in our experience, dramatically improves the quality of the systems we build, in particular their reliability and their flexibility
in response to new requirements"*.
* new applications can force developers into unfamiliar corners.
* they all know there will be changes, they just don't know what changes. They need a process that will help them cop with uncertainty
as their experience grows, to anticipate unanticipated changes.
* iterative development progressively refines the implementation of features in response to feedback until they are good enough.
* we need constant testing to catch regression errors, so we can add new features without breaking existing ones.
* TDD turns testing into a design activity. We use the tests to clarify our ideas about what we want the code to do. It makes
code accessible for testing often drives it towards being cleaner and more modular.
* the cycle of TDD: write a test; write some code to get it working; refactor the code to be as simple an implementation of the tested
features as possible. Repeat.
* we use TDD to give us feedback on the quality of the implementation ("Does it work?") and design ("Is it well structured?").
* writing tests:
  * makes us clarify the acceptance criteria for the next piece of work. We have to ask ourselves how we can tell when we're done.
  * encourages us to write loosely coupled components, so they can easily be tested in isolation and, at higher levels, combined together (design)
  * adds an executable description of what the code does
  * adds to a complete regression suite (implementation)
* running tests:
  * detects errors while the context is fresh in our mind (implementation);
  * let us know when we've done enough, discouraging "gold plating" and unnecessary features (design)
* never write new functionality without a failing test.
* they use acceptance tests (that exercises the system end-to-end in a environment that
tries to reflect the production environment), but personally I would no write a new acceptance test for each
new feature. I prefer writing this kind of test for journeys and invest in other kind of tests (like integration and contract tests).
* **acceptance tests**: does the whole system work? It helps to speak with the domain experts (or Product Owners) and to make sure
that existing features haven't broken.
* **integration**: does our code work against code we can't change?
* **unit**: Do our objects do the right thing/Are they convenient to work with?
* **external quality** and **internal quality**: external quality is how well the sytem meets the needs of its customers and users
(is it functional, reliable, available, responsive, etc.). And internal quality is how well it meets the needs of its developers and
administrators (is it easy to understand, easy to change, etc).
* end to end tests tells about the external quality and how the team understand the domain.
* a class to be easy to unit-test must have explicit dependencies that can easily be substituted and clear responsibilities that
can easily be invoked and verified. Consequently loosely coupled and highly cohesive.
* we're tempted not to write tests when our code makes it difficult, but we try to resist. We use such difficulties as an opportunity
to investigate why the test is hard to write and refactor the code to improve its structure. We call this "listening to the tests".
* elements are coupled if a change in one forces a change in the other.
* an element's cohesion is a measure of whether its responsibilities form a meaningful unit. For example, a class that parses
both dates and URLs is not coherent, because they're unrelated concepts. Think of a machine that washes both clothes and dishes - it's
unlikely to do both well.

## 2. Test-Driven Development with Objects

* object-oriented design focuses more on the communication between objects than on the objects themselves.
* an object communicates by messages: it receives messages from other objects and reacts by sending messages to other objects as well,
as perhaps, returning a value or exception to the original sender. An object has a method of handling every type of message
that it understands and, in most cases, encapsulates some internal state that it uses to coordinate its communication with other objects.
* this lets us change the behavior of the system by changing the composition of its objects-add and removing instances, plugging
different combinations together-rather than writing procedural code. The code we write to manage this composition is a declarative
definition of the how the web of objects will behave. It's easier to change the sytem's behavior because we can focus on
what we want it do, not how.
* **tell, don't ask**: use this "style" to avoid train wreck, where a series of getters is chained together like the carriages
in a train.
* we should ask to the object the question we really want answered, instead of asking for the information to help us figure out
the answer ourselves.
* adding a query method moves the behavior to the most appropriate object, gives it an explanatory name, and makes it easier to test.

### unit-testing the collaborating objects

* a circle object will send messages to one or more of its three neighbors when invoked. How can we test that it does so correctly
without exposing any of its internal state?
* one option is to replace the target object's neighbors in a test with substitutes, or mock objects. We can specify how we expect
the target object to communicate with its mock neighbors for a triggering event; we call these specifications `expectations`. During
the test, the mock objects assert that they have been called as expected; they also implement any stubbed behavior needed to make the
rest of the test work.
* make clear the intention of every test: distinguish between the tested functionality, the supporting infrastructure, and the object
structure.
* JUnit uses reflection to walk the structure of a class and run whatever it can find in that class that represents a test.
* test fixtures: a fixture may be set up before the test runs and torn down after it has finished. Many tests don't need 
tear down and it's enough to just let the JVM garbage work.
* test runners: in charge of running the tests. Per example: `Parameterized` test runner let you write data-driven tests in which
the same test methods are run for many different data values returned from a static method.

## 3. Kick-starting the Test-Driven Cycle

* to run an initial end-to-end test that fails is already a lot of work. Deploying and testing right from the start of a project
forces the team to understand how their system fits into the world. It flushes out the "unknown unknown".
* we view feedback as a fundamental tool, and we want to know as early as possible whether we're moving in the right direction. 
Once we have our fist test in place, the subsequent tests will be much quicker to write.
* one of the symptons of an unstable development environment is that there's no obvious first place to look when something fails.
* a `walking skeleton` is an implementation of the thinnest possible slice of real functionality that we can automatically build,
deploy, and test end-to-end. "End-to-end" refers to the process, as well as the system. We want our test to start from scratch,
build a deployable system, deploy it into a production-like environment, and then run the tests through the deployed system.
* *nothing forces us to understand a process better than trying to automate it*. If it's going to take six weeks to take six
weeks and four signatures to set up a database, we want to know now, not two weeks before delivery.
* we make the smallest number of decisions we can to kick-start the TDD cycle, to allow us to start learning and improving from real
feedback.
* no tests are perfect, of course, but in practice we've found that a substantial test suite allows us to make major changes safely.
* the time to implement the first few features will be unpredictable as the team discovers more about its requirements and target environment.
* incremental development can be disconcerting for teams and management who aren't used to it because it front-loads the stress in a project.
Projects with late integration start calmly but generally turn difficult towards the end as the team tries to pull the system together for
the first time. Late integration is unpredictable because the team has to assemble a great many moving parts with limited time and budget
to fix any failures. The result is that experienced stakeholders react badly to the instability at the start of an incremental project
because they expect that the end of the project will be much worse. Our experience is that a well-run incremental development runs in the
opposite direction. It starts unsettled but then, after a few features have been implemented and the project automation has been built up,
settles in to a routine. As project approaches delivery, the end-game should be a steady production of functionality, perhaps with
a burst of activity before the first release. All the mundane but brittle tasks, such as deployment and upgrades, will have been
automated so that they "just work".  

## 4. Maintaining the Test-Driven Cycle

* writing the tests before coding makes us clarify what we want to achieve. It makes us look at the system from the user's
point of view, understanding what they need it to do rather than speculating about features from the implementers' point of view.
* we prefer to start by testing the simplest success case. Once that's working, we'll have a better idea of the real structure
of the solution and can prioritize between handling any possible failures we noticed along the way and further success cases. Of course,
a feature isn't complete until it's robust. This isn't an excuse not to bother with failure handling-but we can choose when
we want to implement first.
* we discover that these objects need supporting services from the rest of the system to perform their responsibilities. We write more
objects to implement these services, and discover what services these new objects need in turn.
* when we find a feature that's difficult to test, we don't just ask ourselves how to test it, but also why is it difficult to test.
The same structure that makes the code difficult to test now will make it difficult to change in the future.
* if we just test at too fine a grain, we'll miss problems that arise from objects not working together.
* how much unit testing should we do, using mock objects to break external dependencies, and how much integration testing? We
don't think there's a single answer to this question. It depends too much on the context of the team.
* The best we can get from the testing part of TDD, is the confidence that we can change the code without breaking it: Fear kills
progress. The trick is to make sure that the confidence is justified.

## 5. Object-Oriented Style

* we value code that is easy to maintain over code that is easy to write. Implementing a feature in the most direct way can damage
the maintainability of the system, for example by making the code difficult to understand or by introducing hidden dependencies
between components. Balancing immediate and longer-term concerns is often tricky, but we've seen too many teams that can no long
deliver because their system is too brittle.
* change as little code as possible. If all the relevant changes are in one area of code, we don't have to hunt around the system
to get the job done.
* high level abstractions: that's why most people order food from a menu in terms of dishes, rather than detail the recipes used to create them.
* **ports and adapters**: the code for the business domain is isolated from its dependencies on technical infrastructure, such
as databases and user interfaces. We don't want technical concepts to leak into the application model, so we write interfaces
to describe its relationships with the outside world in its terminology.
* features without side effects mean that we can assemble our code from smaller components, minimizing the amount of
risky shared state. We find that a little immutability within the implementation of a class leads to much safer code and that,
if we do a good job, the code reads well too.
* our heuristic is that we should be able to describe what an object does without using any conjunctions ("and", "or").
* the API of a composite object should not be more complicated than that of any of its components.

## 6. Achieving Object-Oriented Design

* first, starting with a test means that we have to describe what we want to achieve before we consider how. This focus
helps us maintain the right level of abstraction for the target object. If the intention of the unit test is unclear then
we're probably mixing up concepts and not ready to start coding. It also helps us with information hiding as we have to
decide what needs to be visible from outside the object. Third, to construct an object for a unit test, we have to pass its
dependencies to it, which means that we have to know what they are. This encourages context independence, since we have
to be able to set up the target object's environment before we can unit-test it. A unit test is just another context. We'll
notice that an object with implicit (or just too many) dependencies is painful to prepare for testing and make a point of cleaning
it up.
* we think the communication patterns between objects are more important.
* an interface describes whether two components will fit together, while a protocol describes whether they will work together.
* we use tdd wih mock objects as a technique to make these communication protocols visible, both as a tool for discovering them
during development and as a description when revisiting the code.
* when starting a new area of code, we might temporarily suspend our design judgment and just write code without attempting to impose
much structure. This allows us to gain some experience in the area and test our understanding of any external APIs we're developing
against. Splitting out a new object also forces us to look at the dependencies of the code we're pulling out.
* under time pressure, it's tempting to leave the unstructured code as is and move on to the next thing ("after all, it works and
it's just one class..."). We've seen too much code where the intention wasn't clear and the cost of cleanup kicked in when the
team could least afford it.
* we might be adding behavior to an object and find that, following our design principles, some new feature doesn't belong inside it.
* we give the new service a name and mock it out in the client object's unit tests, to clarify the relationship between the two. Then
we write an object to provide that service and, in doing so, discover what services that object needs. We follow this chain (or perhaps
a directed graph) of collaborator relationships until we connect up to existing objects, either our own or from a third-party API.
* when the test for an object becomes too complicated to set up when there are too many moving parts to get the code into the relevant
state, consider bundling up some of the collaborating objects.
* we use interfaces to name the roles that objects can play and to describe the messages they'll accept.
* the fewer methods there are on an interface, the more obvious is its role in the calling object. We don't have to worry which
other methods are relevant to a particular call and which were included for convenience. Narrow interfaces are also easier
to write adapters and decorators for; there's less to implement, so it's easier to write objects that compose together well.
* driving an interface from its client avoids leaking excess information about its implementers, which minimizes any
implicit coupling between objects and so keeps the code malleable.
* if there really isn't a good implementation name, it might mean that the interface is poorly named or designed. Perhaps it's
unfocused because it has too many responsibilities; or it's named after its implementation rather than its role in the client.

## 7. Building on Third-Party code

* only mock types that you own - don't mock types you can't change
* although we know how we want our abstraction to behave, we don't know if it really does so until we test it in combination with the third-party code.
* providing mock implementations of third-party types is of limited use when unit-testing the objects that call them.
* a second risk is that we have to be sure that the behavior we stub or mock matches what the external library will actually do. How difficult this is
depends on the quality of the library. Whether it's specified (and implemented) well enough for us to be certain that our unit tests are valid.
Even if we get it right once, we have to make sure that the tests remain valid when we upgrade the libraries.

### write an adapter layer

* if we don't want to mock an external API, how can we test the code that drives it? We will have used TDD to design interfaces for the services our
objects need -- which will be defined in terms of our objects domain, not the external library.
* we write a layer of adapter objects that uses the third party API to implement these interfaces. We keep this layer as thin as possible, to minimize
the amount of potentially brittle and hard-to-test code. We test these adapters with focused integration tests to confirm our understanding of how the
third party API works. There will be relatively few integration tests compared to the number of unit tests, so they should not get in the way of the build
even if they're not as fast as the in-memory unit tests.
* following this approach consistently produces a set of interfaces that define the relationship between our application and the rest of the world
in our application's terms and discourages low-level technical concepts from leaking into the application domain model.
* there are some exceptions where mocking third-party libraries can be helpful. We might use mocks to simulate behavior that is hard to trigger with
the real library, such as throwing exceptions.

## 8. A Worked Example

* even the best people misunderstand requirements and technologies or, sometimes, just miss the point. A resilient process allows
for mistakes and includes techniques for discovering and recovering from errors as early as possible.
* focus on just the immediate change at hand and merging changes back in is never a crisis.
* we've often found it worth writing a small amount of ugly code and seeing how it falls out. It helps us to test our ideas
before we've gone too far, and sometimes the results can be surprising. The important point is to make sure we don't leave it ugly.
* there are teams that overdo their design effort, but our experience is that most teams spend too little time clarifying the code
and pay for it in maintenance overhead.
* we try to minimize the time when we have code that does not compile by keeping changes incremental.
* we're grow our a design from what looks like an unpromising start. We alternate, more or less, between adding features
and reflecting on - and cleaning up - the code that results. The cleaning up stage is essential, since without it we would end up
with an unmaintainable mess. We're prepared to defer refactoring code if we're not yet clear what to do, confident that we will take
the time when we're ready. In the meantime, we keep our code as clean as possible, moving in small increments and using techniques
such as null implementation to minimize the time when it's broken.
* we're try hard to follow what the code is telling us instead of imposing our preconceptions.
* we discover that some of the choices we made are no longer valid, so we dare to change existing code. We continue to refactor
and sense that a more interesting structure is starting to appear.
* nothing shakes out a design like trying to implement it, and between us we know just a handful of people who are smart enough to get
their designs always right. Our coping mechanism is to get into the critical areas of the code early and to allow ourselves to change
our collective mind when we could do better. We rely on our skills, on taking small steps, and on the tests to protect us when we
make changes.
* deciding when to change the design requires a good sense for tradeoffs, which implies both sensitivity and technical maturity:
"I'm about to repeat this code with minor variations, that seems dull and wasteful" as against "This may not be the right time to
rework this, I don't understand it yet".
* as we learn more about what the structure should be by using the code we've written, we learn more about the names we've chosen
when we work with them.
* draw something when you don't know exactly where to start.
* The skill is in learning how to divide requirements up into incremental slices, always having something working, always adding just
one more feature.

## 9. Listening to the Tests

* when writing a test is hard, it can means that your design can be improved.
* we've found that a object easy to test is also easy to change.
* TDD is also about feedback on the code's internal qualities: the coupling and cohesion of its classes, dependencies that
are explicit or hidden, and effective information hiding.
* we don't just ask ourselves how to test it, but also why is it difficult to test.
* instead of using `Date` directly we introduce `Clock` and pass it into the `Receiver` class.
* tests for supportive logging: protect us from breaking any tools and scripts that other people write to analyze them.
* tests for diagnostic logging: not necessarily tested.
* clients should not be forced to depend upon interfaces that they do not use, but that's exactly what we do when we mock
a concrete class.
* interfaces give us better information-hiding and clearer abstractions.
* once a codebase starts to "rot", the developers will be under pressure to botch the code to get the next job done. It
doesn't take many such episodes to dissipate a team's good intentions.
* test names should act as documentation for the code.
* AAA: arrange, act and assert.
* all code should emphasize **"what"** it does over **"how"**.
* our highest concern is making the test describe what the target code does, so we refactor enough to be able to see its
flow.
* write test data builders
* allocate literal values to variables and constants with names that describe their function.

## 10. Test Diagnostics

* a "failing" test has succeeded at the job it was design to do, but it needs to be clear about the failure's reason.
* sometimes is quicker to roll back and restart with a clear head than to keep digging.
* the tests overspecify the expected behavior of the target code, constraining it more than necessary.
* mind methods returning null to other objects since null can be used as a return of anything. So if you're mocking something
make sure that is returning the right thing rather than just forcing a "null" return.
* use expressive assertions.
* commands have side effects, queries don't.
* high level tests to make sure that everything works together.
* make sure every test starts with the database in a known, clean state.
* ORM mappings can by declarative or configuration, but misconfiguration creates defects that are difficult to
diagnose, so we use round-trip tests so we can quickly identify the cause of such defects.
* tests with realistic infrastructure are slower than with in memory database. We also can unit-test our code
by defining a clean interface to the persistence infrastructure.
