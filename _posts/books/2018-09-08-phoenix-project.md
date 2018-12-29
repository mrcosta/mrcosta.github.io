---
layout: post
title: "The Phoenix Project"
date: 2018-09-08 17:00:00 
comments: true
description: "Review of The Phoenix Project"
categories:
- books
- devops
- agile
permalink: 2018-09-08-phoenix-project

---

![Image]({{ site.url }}/assets/images/books/phoenix-project.jpg)

# **What the book is about?**

The story goes in an IT Department of a large corporation. There, developers are facing everyday issues of a company that doesn't have any kind of plan or process for a critical project. People are trying to do their best to overcome the problems, but sometimes just getting the work done is not the smartest way. You need to think about how you are going to do the work. The book shows the problems when you don't talk about work (plan or think strategically) and instead, you just do the work.

# **The book**

Since the book is like a novel about people working in the department, I prefer to enumerate the parts that I learned something:

* **"The relationship between IT and the business is like a dysfunctional marriage -- both feel powerless and held hostage by the other."**: An example is the business asking for changes that were not planned while developers team is trying to overcome the most critical technical debts to delivery functionality faster.

* **The company needs to have its priorities**: it's not just a matter to put 3 critical projects at the same time and consider that they're all priority 0.

* **Having someone like a Jack of all trades is kind of dangerous (for the company and for the person)**: the person in this role is getting smarter and the entire system dumber (dependent on this person and creating a kind of blocker when this person is not around).

* **And processes were supposed to protect people**: work needs to be shared, otherwise "Jack" would never take holidays, per example.
 
* **we need to talk about work**: people tend to think that meetings and processes are waste of time and that just actually doing the work is the way to achieve something. Well organized meetings can help you brainstorm solutions and have people aligned in what needs to be done. Processes help the team control and follow what, and sometimes how, needs to be done. In some situations, we tend just to react and forget that analyzing a situation before acting on it is important to check if we can solve the real problem with the best solution.

* **track the work in progress**: in the context of the book, where they had a lot of different messy systems and different teams, this was crucial to control what changes and when would be deployed in production environments. If you are tracking this, probably you're tracking other steps, and this is important to make sure that you're not skipping steps that would guarantee the quality of your delivery.

* **"Improving daily work is even more important than doing daily work**" or **"People think just because it doesn't oil and doesn't delivery physical packages that it doesn't need preventive maintenance. The work is invisible."** and even more like **"If you are not improving, entropy guarantees that you are actually getting worse."**: in our day to day is easy to get yourself repeating boring tasks to solve a problem or to access some specific environment. It's important to be aware of this small tasks that you're repeatedly doing and try to automate in order to improve your way to work to focus on delivering things that are focused on the business. Besides this, any kind of technical debt or experimentation (tool or practices) that could improve the project somehow.

* the four types of work: This I will copy from [Keyvan Akbary](https://github.com/keyvanakbary/book-notes/blob/master/the-phoenix-project.md) because he resumed very well: 
  * **Business projects**: these are business initiatives.
  * **Internal IT projects**: infrastructure or IT Operations projects that business projects might create, as well as internally generated improvement projects. This creates a problem when IT Operations is a bottleneck because there is no easy way to find out who much of capacity is already committed to internal projects.
  * **Changes**: generated from the previous two types of work.
  * **Unplanned word or recovery work**: operational incidents and problems. Always come at the expense of other planned work commitments.

* **some values that a DevOps culture should bring**: stability, security, scalability, manageability, operability, and continuity.

* **"it's like the free puppy. Its not the upfront capital that kills you, its the operations and maintenance on the backend"**: make sure that the easy solution that you are going to accept (implementation that you are doing or the third party software that you are buying) it's also the best one on the long-term and it will not make you spend your resources again in something that should be already considered solved.

# **Conclusion**

One of the objectives of the book is to motivate a learning culture in the company. Per example, when accidents occur, the response to those accidents is  to maximize opportunities for organizational learning – no naming, blaming and shaming the person who caused the failure. 

And for me, it was also good to get to know some other problems and indication of problems that a team could face. This way is easier to identify them and convince people why they should adopt such a practice or use some tool in order to improve.
