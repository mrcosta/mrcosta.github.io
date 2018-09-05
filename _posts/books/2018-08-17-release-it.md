---
layout: post
title: "Release It!: Design and Deploy Production-Ready Software"
date: 2018-08-17 11:00:00 
comments: true
description: "Review of Release It"
categories:
- books
- application's resiliency
- monitoring in production
permalink: 2018-08-17-release-it

---

![Image]({{ site.url }}/assets/images/books/release-it.jpg)

# **What this book is about it?**

We don't learn what a software should **NOT** do. When we finish a certain feature, if everything goes well during the QA, then it's just a matter to deploy it in production and then our work is finally done. 

We forgot that running an application in production is totally different than running in a QA environment, where the number of requests is low and maybe can be restarted from time to time to avoid memory issues. In production the users are crazy, they can find out any crazy edge case that you never fought about. The system will have a totally different load and will be running for days and then we will figure out that in fact that "easy" to implement feature that you did it's causing a memory leak and you will take the whole day until you figure out which exactly line inside your perfect single responsibility class that has test coverage by 100% is causing the problem.

The message of the book is clear: problems will appear with your application in production. But what can you do to prevent or at least to be more prepared to face them when the time comes? Let's check.

# **The book**

The book sometimes is direct to the point about the problems that you're going to face, but also Michael tells some real-life stories from his experience to set the ground before going to the point. I will show some of learnings and questions that I got from this book:

When something goes wrong, restoring the service take precedence over the investigation. Of course is going to depend on what kind of application is yours, but usually, you should focus to bring it back working to your users.
What you can try to do, is to have some scripts in hand to have a post-mortem analysis to understand the problem and avoid it again. Connect to this, is that you need to be aware of the percentage uptime of your application. This probably it will dictate how fast you need to act to fix the problem.

You are focused on your business requirements and the features that will deliver in the next days, but are you aware if in somehow this is going to bring more users? 
* or, if is it going to happen a marketing action that is going to bring maybe the triple of users? 
* if yes, are you prepared to handle all of them? 
* what are the things that you need to scale in your application? 
* is just the number of servers or you also need to increase the number of database connections? 
* is better to your application/company increase the number of servers or the quality of each server?
* is your third-party partner also prepared for the avalanche of requests that will happen?

## **About interaction with external services**:

* have you done any kind of load testing to know how many requests per second/minute your third party can handle? (use data from production to test performance)
* do you have a circuit breaker in place to handle all these requests and its responses?
* can you implement some kind of cache in order to improve performance? if yes, have you thought in how cache invalidation should work?
* slow responses are worse than no response. Check for resources to check if they are online (use circuit breaker or health monitoring for it)
* validate mandatory information that is necessary for transaction fast. Don't call some resource before validating that the provided parameters are correct for all the necessary and different calls
* threat well your exceptions and close connections.
* if the third party is having problems, use timeout pattern (try, wait and if doesn't work, keep moving). If this keeps happening, queue the operation and try in a different moment

## **Tips**

### In order to provide a better user experience:

* cache static content
* improve response removing extra chars and unnecessary info (whitespaces and JSON payload per example)
* database improvements: queries, indexes and connection pools
* in some moment your database will have a lot of unnecessary data and will be necessary to scale ou to clean it because probably will make your application slow in somehow
* always give a response to the user
* decouple your dependencies. Your service still needs to operate even if the partners are down. Design it in a way that if something is not working, the rest of your system can still be functional

### In order to have a better work environment/system design that facilitates to do your work:

* have environments similar as possible to production
* logging
* implement a way to track back all the requests that go into different services (trackId)
* monitor your application at different levels: you can get to a point that even the logger will not work to you easily know the problem.
* organizations are constrained to produce designs whose structure are copies of the communication structures of these organizations. So be aware of how communication flows in your company to make sure that you are solving the right problem with the right solution
* avoid applications communicating directly to each other (use queues)
* beware with concurrency in your system and where it can cause problems

### When debugging:

* thread dumps can help you with the problem

## **Conclusion**

I learned a lot from this book. Today I'm a little bit aware of things going wrong in production and to ask better questions that it will happen when thinking in other aspects of the solution that I'm providing. Do I recommend the book? If you are an intermediary developer (with some years of career), yes. It will open your mind to some crazy problems that probably you will face in a near future. If you are more experience probably you already learn from your previous experiences most of the content of this book.

