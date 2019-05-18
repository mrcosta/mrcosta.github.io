---
layout: post
title: "Notes from Accelerate: Building and Scaling High Performing Technology Organizations"
date: 2019-05-01 11:00:00 
comments: true
description: "Notes from Accelerate: Building and Scaling High Performing Technology Organizations"
categories:
- books
- devops
- practices
- culture
permalink: 2019-05-01-accelerate-devops

---

<p align="center">
    <img src="{{ site.url}}/assets/images/books/accellerate-devops.jpg" alt="accelerate devops" width="200"/>
</p>

# **1. Some thoughts**

It's good to work in projects that use the latest practices applied by the industry, but sometimes that is not the case
and you have to sell this fish to your boss in order to change organization. Sometimes just changing processes for the
sake of trying to make the company is not enough, so have numbers and a whole book to prove their point with numbers
is something that makes you even more confident to purpose something at your workspace.  

# **2. What they found**

* velocity is designed to be used as a capacity planning tool (how long it will take to finish the tasks);
* the measure should focus on outcomes, not output
* we should measure: delivery lead time, deployment frequency, time to restore service and change fail rate.
    * delivery part of the lead time: the time it takes for work to be implemented, tested, and delivered.
    * product delivery lead time: the time it takes to go from code committed to code successfully running in production.
    * deployment frequency: a software deployment to production or to an app store.
    * how quickly can a service be restored?
    * what percentage of changes to production fail
    
* software delivery performance impacts organizational performance and noncommercial performance.
* the fact that software delivery performance matters provides a strong argument against outsourcing the development of software
that is strategic to your business.

# **3. Measuring Culture**

* basic assumptions are formed over time as members of a group or organizations make sense of relationships, 
events, and activities. And are things that we may find difficult to articulate.
* values also influence group interactions and activities by establishing social norms, which shape
the actions of the group members and provide contextual rules.
* **pathological** organizations are characterized by large amounts of fear and threat.
* **bureaucratic** organizations protect departments. Those in the department want to maintain their
"turf", insist on their rules, and generally do things by the book - their book.
* **generative** organizations focus on the mission. How do we accomplish our goal? Everything is 
subordinated to good performance, to doing what we are supposed to do.

Three characteristics of good information:
* it provides answers to the questions that the receiver needs to be answered
* it's timely
* it is presented in such a way that it can be effectively used by the receiver

A good culture requires trust and cooperation, so it reflects the level of collaboration
and trust inside the organization. You need to have good information to make decisions and these
decisions can be easily reverted with the team is open and transparent rather than closed and hierarchical.

Google found that "who is on a team matter less than how the team members interact, structure their work,
and view their contributions". It comes down to team dynamics.

In complex adaptive systems, accidents are almost never the fault of a single person. Rather, accidents
typically emerge from a complex interplay of contributing factors.

To change the culture: start by changing how people behave, what they do.

# **4. Technical Practices**

Many Agile adoptions have treated technical practices as secondary as compared to the management and team practices. The
research shows that technical practices play a vital role in achieving these outcomes. The research also found that Continuous
Delivery has an impact on software delivery performance, organizational culture, and other outcome measures, such as team burnout
and deployment pain.

**Continuous Delivery**: 

Continuous Delivery is a set of capabilities that enable us to get changes of all kinds into production safely, quickly and 
sustainably:
* detect any issues quickly
* cost of pushing individual changes is very low
* should be possible to provide our environments from information stored in version control
* when implemented correctly, the process of releasing new versions to users should be a routine activity that can be performed
on demand at any time.
* reduce deployment pain and team burnout because it makes not to be a big-bang event every time that needs to be done and
can be done during working hours
* teams that can choose their own tools based on what is best for the users of those tools

They found that teams that did well at continuous delivery achieved the following outcomes:
* strong identification with the organization your work for
* a higher level of software delivery performance
* lower change fail rates
* a generative, performance-oriented culture
This means that investments in technology are also investments in people

### 4.1 The impacts of Continuous Delivery on Quality

* in the quality and performance of applications
* the percentage of time spent on rework or unplanned work
* the percentage of time spent working on defects identified by the end users (it represents the quality)

**Unplanned work**: "paying attention to the low fuel warning light on an automobile versus running out of gas on the highway".
In the first case, the organization can fix the problem in a planned manner, without much urgency or disruption to other
scheduled work. In the second case, they must fix the problem in a highly urgent manner, often requiring all hands on deck --
for example, have six engineers drop everything and run down the highway with full gas cans to refuel a stranded truck.

**Test data management**: successful teams had adequate test data to run their fully automated test suites and could acquire
test data for running automated tests on demand.

**Trunk Based Development**: it was correlated more with high delivery performance. Teams had fewer than three active
branches. Teams working with short-lived branches perform better than the ones with longer-lived branches.

# **5. Architecture**

Employing the latest whizzy microservices architecture deployed on containers is no guarantee of higher
performance if you ignore these characteristics.

High performers:
  * we can do most of our testing without requiring an integrated environment
  * we can and do deploy or release our application independently of other applications/services it depends on

The biggest contributors to continuous delivery:
  * make large-scale changes without the permission of somebody outside the team
  * make large-scale changes without other teams having to change something
  * complete their work without telling other people from external teams
  * deploy and release the product without depending on others
  * testing on demand
  * perform deployment during normal business hours
In other words: architecture and teams are loosely coupled.

When teams can decide which tools they use, it contributes to software delivery performance and, in turn, to organizational
performance. When the tools provided actually make life easier for the engineers who use them, they will adopt
them of their own free will. This is a much better approach than forcing them to use tools that have been chosen for the
convenience of other stakeholders.

# **6. Integrating infosec into the delivery lifecycle**

Build security from scratch, when is the best moment to do this kind of preparation. Building
security improves delivery performance because you spent less time remediating security
issues.

# **7. Management practices for software**

* limit work in progress
* visual displays with productivity metrics and aligning them with operational goals
* use data from the application to make business decisions

high performers: peer review (with the team) instead of bureaucratic process with external entities.

# **8. Product Development**

* user research from the very beginning of the product lifecycle.
* use of MVPs
* customer feedback
* work (features) in small batches
* update specifications without requiring the approval of people outside the team

# **9. Making Work Sustainable**

**Deployment pain**: the fear and anxiety that engineers can feel when deploying something in production.
They find that where code deployments are most painful, you'll find the poorest software delivery performance,
organizational performance, and culture. Deployments need to easily happen during normal business hours.
Build intelligence into the application and the platform so that the deployment process can be as simple as possible.

**Burnout**:
* "learning from failure" kind of environment
* communicating a strong sense of purpose
* investing in employee development
* asking employees what is preventing them from achieving their objectives
* giving employees time, space, and resources to experiment and learn

**Common problems that can lead to burnout**:

* job demands exceed human limits
* lack of control: inability to influence decisions that affect your job
* insufficient rewards: financial, institutional, or social rewards
* breakdown of community: unsupportive workplace environment
* absence of fairness: lack of fairness in decision-making processes

**How to reduce or fight burnout**:

* strong feelings of burnout are found in organizations with a pathological, power-oriented culture. Create
a blame-free environment, striving to learn from failures, and communicating a shared sense of purpose.
* deployment pain
* the effectiveness of leaders
* organizational investments in DevOps
* organizational performance. When organizational values and individual values aren't aligned, you are more
likely to see burnout in employees.

# **10. Employee satisfaction, identity, and engagement**

They found that employees in high-performing teams were 2.2 times more likely to recommend their organization
to a friend: highly engaged workers are associated with the high performers' companies.

When leaders invest in their best work, employees identify more strongly with the organization and are willing to go the
extra mile to help it be successful. In return, organizations get higher levels of performance and productivity,
which lead to better outcomes for the business.

```
Continuous Delivery  
                    --> Job Satisfaction --> Organisational Performance
Lean Practices
```

# **11. Transformational Leadership**

Good leaders affect a team's ability to deliver code, architect good systems, and apply Lean principles to
how the team manages its work and develops products:

* establishing and supporting generative and high-trust cultural norms
* creating technologies that enable developer productivity, reducing code deployment lead times and supporting more
reliable infrastructures
* supporting team experimentation and innovation, and creating and implementing better products faster
* working across organizational silos to achieve strategic alignment

Leaders are those who set the tone of the organization and reinforce the desired cultural norms.

**Five characteristics of a transformational leader are**
* vision: understands where the organisation is going and where it should be
* inspirational communication
* intellectual stimulation: challenges followers to think about problems in new ways
* supportive leadership: demonstrates care and consideration of followers personal needs and feelings
* personal recognition: praises and acknowledges the achievement of goals and improvements in work quality; personally
compliments others when they do outstanding work.

### 11.1 Other things to create a good environment

* establish a dedicated training budget and make sure people know about it
* encourage staff to attend technical conferences at least once a year and summarize what they learned for the team
* set-up internal hack days
* set-up days to work on technical debt
* set-up space to share knowledge
* give time to experiment with new technologies
* let the team choose their tools
* make monitoring a priority: proactive monitoring was strongly related to performance and job satisfaction.