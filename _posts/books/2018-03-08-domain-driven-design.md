---
layout: post
title: "Domain Driven Design: Tackling Complexity in the Heart of Software"
date: 2018-03-08 11:00:00 
comments: true
description: "Review of Domain Driven Design"
categories:
- books
- domain driven design 
- good practices 
permalink: 2018-03-08-domain-driven-design
---

![Image]({{ site.url }}/assets/images/books/ddd.jpg)

# **What the book is about?**

Applications are complex, solving real-world problems is complex. Give 1 problem to 10 different people and then you will have 10 different solutions, that probably have the same results. The idea of the book is to present some ideas on how to organize and approach to solve real-world problems. Is not necessarily about architecture, but more into how you can see the problem and represent in a way that developers and business people can understand each other better.

# **Why I read this book?**

I really believe that clean and understandable code have a big role when developing systems. Reading code is one of the most important activities of a developer's daily and always present when
modifying or adding a new piece of code.

The aim of the book is to purpose an approach where your real-world problem is reflected in your codebase. In this way, you have one common language (called Ubiquitous Language) between all the people involved in the project. 
Then, to understand how to do this in a better way it was for sure a wish that I had a long time ago.

# **The book**

I had luck about reading this book: I can easily affirm that in most of the places that I worked people were pushing to a more DDD approach to be applied in our codebase even not knowing all the details about the subject.

The first part of the book is basically explaining how you can associate the model from the real world with your code design and mix with the technical requirements that you can face in a common application (like querying a database, calling an external API and, of course, the business logic itself).

The second part is based on some examples showing some refactors and different ways to model some problems. He also explains some ideas when integrating between different teams that maybe can use the same design in their respective applications.

Eric is very pragmatic in this book. He knows that projects are different, deadlines and resources are different, and if is not possible to put the same effort to build a rich and understandable design for the whole application, at least choose a **main domain** (the core of your application, when changes usually are more frequent).

The main message to me is that you need to put more effort in describing your codebase as nearest possible to the real world. Why? Because it's easier to understand the real world, consequentially easier to read the code. because *" a system that is hard to understand, is hard to change"*.


