---
layout: post
title: "Notes from Building Microservices"
date: 2019-01-03 11:00:00 
comments: true
description: "Notes from Building Microservices"
categories:
- books
- architecture/design
- microservices
permalink: 2019-01-03-building-microservices

---

<p align="center">
    <img src="{{ site.url}}/assets/images/books/building_microservices.jpg" alt="building microservices" width="200"/>
</p>

## 1. Microservices

* Domain-driven design, continuous delivery, on demand virtualization, infrastructure automation, small autonomous teams and
systems at scale: microservices have emerged from this world.
* in a monolithic system we try to make sure that our code is cohesive, trying to group code together (single responsibility principle).
* small and autonomous services that work together.
* microservices is also focus in high cohesion. It focus service boundaries on business boundaries, making it obvious
where code lives for a given piece of functionality. And keeping it focused we avoid the temptation to grow it large.
* the rule of thumb gave in the book is that a microservice could be rewritten in two weeks.
* if a team is small, breakdown into microservices can be very sensible
* microservice it's a separate entity. We try to avoid packing multiple services onto the same "machine"
* all communication between the services themselves are via network calls, to enforce separation between the services and tight coupling.
* these services need to be able to change independently of each other, and be deployed by themselves without requiring consumers to change.
* our services exposes an application programming interface (API), and collaborating services communicate with us via those APIs.
* **the golden rule**: can you make a change to a service and deploy it by itself without changing anything else?
* if one part of our system needs to improve its performance, we might decide to use a different technology and this is easy to
approach with a microservices architecture than with a monolith.
* **resilience**: if one component of a system fails, but that failures doesn't cascade, you can isolate the problem and the rest of the system
can carry on working.
* **scaling**: we can just scale those services that need scaling, allowing us to run other parts of the system on smaller, less powerful
hardware.
* **ease of deployment**: a one-line change to a big codebase requires the whole application to be deployed.
* **composability**: with microservices, think of us opening up seams in our system that are addressable by outside parties. 
As circumstances change, we can build things in different ways. With our services being in small size, the cost to replace
them with a better implementation, or even delete them altogether is easier to manage.
* you should think of microservices as a specific approach for SOA
* you'll have to get much better at handling deployment, testing, and monitoring to unlock the benefits we've covered so far.

### Shared libraries

* you lose true technology heterogeneity. The library typically has to be in the same language, or at the very least run on the same platform.
* unless you're using dynamically linked libraries, you cannot deploy a new library without redeploying the entire process, so your
ability to deploy changes in isolation is reduced.
* you'll find yourself creating code for common tasks that aren't specific to your business domain that you want to reuse across
the organization, which is an obvious candidate for becoming reusable library.
* shared code used to communicate between services can become a point of coupling.

## 2. The Evolutionary Architect

* the architecture needs to make sure we have a joined-up technical vision, one that should help us deliver the system
our customers need.
* bridges and buildings will fall down much less than the number of times our programs will crash, making comparisons
with engineers quite unfair.
* our architecture needs to shift their thinking away from creating the perfect end product, and instead focus on helping create a
framework in which the right systems can emerge, and continue to grow as we learn more.
* architecture have a duty to ensure that the system is habitable for developers too.
* you may be OK with a team picking a different technology in a area of the system that they own.
* *"be worried about what happens between the boxes, and be liberal in what happens inside"*
* strategic goals should focus in making the customers happy: *"let the customer achieve as much as possible using self-service"*.
* **principles**: rules you have made in order to align with a goal: if a goal is to decrease time to market, so you may define that teams will have full control over the lifecycle, 
independently of any other team.
* **practices**: how we ensure the principles are being carried out: all logs need to be centralized; HTTP/REST is the standard integration style.
* the principles between a Java and a .NET team can be equal, but the practices different. You can have some documentation around principles and practices.

<p align="center">
    <img src="{{ site.url}}/assets/images/books/goals-principles-practices.png" alt="principles and goals" width="400"/>
</p>

* what we need to ensure that one bad service doesn't bring down the whole system?
* we need to find the balance between autonomy of the service without losing sight of the bigger picture.

### Other aspects of microservices

* ensure that all services emit health and general monitoring metrics in the same way (push data into a central location or some polling system).
* if you pick HTTP/REST, are you going to handle pagination and versioning?
* ensure that our services shield themselves from unhealthy, downstream calls.
* write good documentation with examples in order to expose your service.
* what if the developers had a template to implement core attributes that each service needs? (some of the practices already in place).
* if a lot of exceptions happen around the principles/practices, then we can change to reflect a new understand of it.
* *I often take the approach that I should go with the group decision*. Even that this means going with a decision that you don't agree with.
* the team needs to understand the vision themselves.

### How to model services

* *what the world stood? On a tortoise. And what does the tortoise stand? On another tortoise."
* a change to one service should not require a change to another.
* we want to limit the number of calls from one service to another, because beyond the potential performance problem, chatty communication
can lead to tight coupling.
* related behavior sit together and unrelated behavior sit elsewhere.
* each bounded context has an explicit interface. There is an internal-only representation, and the external representation we expose.
* premature decomposing can be costly if you're new to the domain. Break a monolith is easier than going microservices from day one.
* you need to think about bounded contexts from a capability perspective and not about data that is shared.
* if a new field is added to a piece of data, existing costumers shouldn't be impacted.
* any technology that push to expose internal representation should be avoided.
* **shared database**: we're allowing services to view and bind to internal implementation details and tight to all of them to one 
specific technology

### Communication

* synchronous: it blocks the operation until is complete
* synchronous: doesn't wait for the operation be complete and may not even care if it was complete or not
* with an event-based collaboration, we invert things. Instead of a client initiating requests asking for things to be done, it instead says
`this thing happened and expects other parties to know what to do`. You can just add subscribe to these events.
* **orchestration vs choreography**: with orchestration we rely on a central brain to guide and drive the process, much like the conductor
in a orchestra. With choreography, we inform each part ot its job, and let it work out the details.
* you need to be aware of orchestration, because you can end up with `god services` communicating too much with other services.
* with choreography you need extra work to ensure that you can monitor that the right things have happened.
* *I strongly prefer aiming for a choreographed system, where each service is smart enough to understand its role in the whole dance*
* the client has to understand the semantics of the API in much the same way as a human being needs to understand that on a shopping
website the cart is where the items to be purchased will be.
* provide links to allow consumers to navigate in the API endpoints (HAL standard per example).
* for choreography: message brokers. And use dead-letter-queues to retry messages in another time.
* have a good monitoring and consider the use of correlation IDs, which allow you to trace requests across process boundaries.
* rather than make code shared, the company copies it for every new service to ensure that coupling doesn't leak in. The evils
of too much coupling between services are far worse than the problems caused by code duplication. For logging libraries is ok, since
is not related with domain logic.
* the best way to reduce impact of making breaking changes, is to avoid making them in the first place.
* MAJOR change contains incompatible changes; MINOR new functionality but still compatible and PATCH just for bug fixes.
* keep more than one version running and stop it when all the services migrated to the new one.
* the longer it takes to these services to update, you should look to coexist different endpoints in the same microservice.

## 3. Microservices and UI

* users don't want to feel that different parts of the interface work in different ways.
* **backend for frontends**: it can handle multiple backend calls and aggregate content if needed. This can lead to everything
being thrown in together, and suddenly we start to lose isolation of our various user interfaces, limiting our ability to release them
independently. It should contain behavior specific to delivering a particular user experience.

## 4. Splitting the Monolith

* identify the boundaries in our code.
* the packages should interact in the same way the real-life organizational groups in our domain interact.
* split as you go: think about where you are going to get the most benefit from some part of the codebase being separated. Other criterias:
areas that demands more frequent changes; or team location (different countries); technology
* sometimes making one thing slower in exchange for other things is the right thing to do, specially if slower is still acceptable.
* sometimes the system will change because some concept that was not there before had a need and then it was "borned".
* **eventual consistency**: we accept that the system will get itself into a consistent state at some point in the future. Or we can just
revert the transaction and try again.
* be aware of report generation: some calls can require big amounts of data. **Data pumbs**: push data to the report system using the database
of each microservice. We can push to this system as soon we see that an event happened (a user created per example). Or we can go
towards eventing system capable of routing our data to multiple different places depending on need.
* when trying to figure out the impact of a change: what calls get made? Is there any circular references? Do you see two services
that are overly chatty? (this can indicate that they should be just one).

## 5. Deployment

* is good to have the entire path to production to see the quality of our software and to reduce the time
take between releases, as we have one place to observe our build and releases process.
* have pre-baked images with already installed dependencies to run the service.
* install the configuration of the services in the source control.
* the more your configuration changes, the more you will find problems only in certain environments.
* be aware in how to handle sensitive information (credentials per example).
* carefully when deploying different services at the same host. This way you're not 100% making them separated, since an
overload in one service can compromise the resources for the other services. An outage to one host should impact only a single service.
* can you write a line of code to launch a virtual machine, or shut one down? Can you deploy the software you have written
automatically? Can you deploy database changes without manual intervention? Embracing a culture of automation is key if you want
to keep the complexities of microservice architectures in check.
* use containers: one host, multiple containers with one service each.

## 6. Deployment

* you will want tests of different scope for different purposes.
* *"we decide that from a feedback point of view we had way too many service and end-to-end tests"*
* mountbank to stube calls to other systems
* as test scope increases, so too does the number of moving parts. A test can fail because some service is down, and this
is not related to the nature of the test itself. If you have tests that sometimes fails, but everyone just re-runs them because
they may pass again later, then you have flaky tests.
* **normalization of deviance**: the idea that over time we can become so accustomed to things being wrong that we start to accept them
as being normal and not a problem.
* not shared end-to-end tests: everyone is going to assume it is someone else's problem. 
* if the same feature is covered in 20 different tests, perhaps we can get rid of half of them.
* with a long test suite, any breaks take a while to fix. And while a broken integration test stage is being fixed, more changes
from upstream teams can pile in.
* **TEST JOURNEYS, NOT STORIES**. High value interactions and very few in number.
* **consumer-driven tests to the rescue**: we don't want our changes to break consumers. We are defining the expectations of
a consumer on a service (or producer). The expectations of the consumers are captured in code form as tests, which are then run
against the producer. If done right, these CDCs should be run as part of the CI build of the producer, ensuring that it never gets
deployed if it breaks on of these contracts.
* on the producer side, you then verify that this consumer specification is met by using the JSON Pact specification to drive call
against your API and verify responses.
* these end-to-end tests may form a useful safety net, where you are trading off cycle time for decreased risk. But be aware
that you will not eliminate all sources of defects.
* **smoke test suite**: a collection of tests designed to be run against newly deployed software to confirm that the deployment worked.
* **blue green deployment**: by keeping the old version running while we perform our release we greatly reduce the downtime associated
with releasing our software.
* create a good process to revert code from a new deployment and back to a previous version.
* execute performance tests.

## 7. Monitoring

* at bare minimum, monitore the response time of the service
* number of errors
* central aggregations for logs and metrics
* are we reaching out limit? How long until we need more hosts?
* track what features that is never been actually used.
* what is a good cpu level for the application?
* a way to track a call in all the services (as in a stack trace).
* standardize the way that you monitor in your services.

## 8. Security

* authentication: I'm who I'm saying I am.
* principal: who or what is been authenticated.
* authorization: map the action that lets the principal execute it.
* inside the own network, sometimes there is no authentication. But then is easier for an attack to get information once
inside this network.
* use HTTPS
* API keys allow a service to identify who is making a call, and place limits on what they can do.
* application firewall can help throttle connections from certain IP ranges and detect other sorts of malicious attacks.
* how do you revoke access to credentials when someone leaves the organization?

## 9. Other notes and tools

* organizations that can change their structure is likely to have faster time to market.
* *no matter how it looks at first, it's always a people problem*.
* assume that failure, in some aspect, is going to happen.
* how long an operation can last? Can you expect a service to be down? How much data loss is acceptable?
* what happens if "this" is down?
* **fail fast!**. Do not have slow requests at your system. Avoid calling unhealthy systems in the first place.
* use circuit breakers: all requests fails while the circuit breaker is in its blown state. After a certain period of time,
the client sends a few requests through to see if the service has recovered.
* **CQRS**: command-query responsibility segregation: the command and query parts of our system could live in different services,
or different hardware, and could make use of radically different types of data store.
* be conscious about your cache configuration. Don't let the cached data living too long.
* you can autoscale when you see an increase in load or an instance failure.
* use a service to share configuration between multiple nodes.
* zookeeper: general or service specific configuration, allowing you to do tasks like dynamically changing log levels
or turning off features of a running system.

