---
layout: post
title: "You know nothing when debugging"
date: 2018-03-02 22:00:00 
comments: true
description: "A little story about something hard to find"
categories:
- debugging, cronjobs, queues
permalink: 2018-03-02-you-know-nothing-when-debugging
---

# why

In my current project, we had a problem in production that it was hard to track. We spent almost two entire days to understand what happened and to fix the problem. The idea is to give some debugging tips and also to remember this
specific fact for myself from the future.

# the problem

### the flow

Every hour a cronjob sends a message to a queue and the consumer/worker that is subscribed to it processes the message.

### what is processed:

In our domain, we have the model client that has a *contract status* with the following statuses in the following order

```ruby
TO_BE_SENT --> SENT --> ORDER_RECEIVED --> ORDER_IN_PROGRESS --> CONTRACT_READY
```

In order to update the status, we call a third party that will give us the update of our client regarding its contract status.

Based on the new status of the client that we retrieved (if suitable), we build the current client status in our database. During the processing, if something goes wrong, we would just try to execute again with a maximum of attempts limited to 3.

Easy, right? Hum... One of the parameters that we pass to the third party is a *timestamp*, that answers the question: *"do you have any updates for users after this timestamp?"*

And the way that we were building this parameter was based on the previous timestamp used in the previous call (in the previous hour). After a successfull processing we would store this timestamp in the databased to be used as a reference for the next call.

## what went wrong

Users contract status were not been updated properly. What, where exactly, how and why this was happenning?

# debugging

When you start debugging something, one of the first things that you need to know is what it's been done in that piece of code: 

* what are the business rules that are behind that process?
* what is the complexity of this code in technical terms? is it using something from the language/tool that you know? is it an advanced feature?
* which states is it changing?
* what are the results returned?
* during this processing, what is it been used: databases, cronjobs, interceptors, external requests?

## debugging that piece of code

In my opinion, based on the explanation I gave above about what was been processed, it seems not complex, right? In the end would be just an update in a state of a model in the database. So I thought I could easily debug this.

I was so innocent about it:

* first I assume that I knew enough about queues, messages and retries
* I also thought that I already mastered how to handle transactions (abort or commit some operation)

And that's where I think *"that piece of code"* is not just a piece of code. You need to know, when debugging, line by line what's happening. Don't underestimate the things that are being executed. In first place if you are debugging it, it's because you didn't find the root cause just taking a very briefly look at it.

In the end it's up to you to focus in a very specific part of the code because you know the code very well, you are experienced, you implemented that part of the application and so on. **BUT, if after sometime you still didn't find the cause of the problem, stop assuming how things are working and to see for yourself it happening**. It's not that you believe that things are working differently now, but it's just that you need to learn them again and do your new judgment about it. And again, please, be pragmatic when reading this statement. It's you that are debugging.

## but why learn everything again?

I was watching the logs for this processment and it has been executed one, two, three times and then, in the end, we saw an exception been throw related to parse a user from the database to our model.

my mistakes:

* I assume that process was happening three times (and yes, it was)
* I assume that only in the third one the exception was happening
* since I considered that the two first executions was done, I thought that problem was because it was executing more than one time, instead of a single processing

Based on my previous experience and in the way I interpreted the code, I completely assume a different behavior for the code than the one that in fact was happening. **My learning here: be sure in how things are connected and working**.

And why I was wrong? Because in the end the code was failing in the first, and in the second and, of course, in the third execution. The problem was that the exception was been holded until the very last attempt before been throw. And this completely makes sense and I assume a totally different behavior in how the process was working.

The exception? The user information from the database was not been parsed as the proper user to our domain.

## one mistake is not enough

The reason for why this was happening?

Well, pretty simple and, again, pretty hard to guess (at least for me). The user has a salutation that can assume one of the given values: **HERR** or **FRAU** (in German)

This values in our domain are expressed as an enum. The exception that we caught was basically complaining that it couldn't parse the value `Herr` to one of the expected values listed above.

Ok, seems fair, since our deserializer was not customized to read `Herr` and we thought that no one would manually change these values in the database.

Conclusion: someone for sure changed the database and this value for some user. (This is another history why they can change the database). So let's check:

```ruby
SELECT distinct(salutation) FROM user
```

The results that were retrieved was always `HERR` and `FRAU`. Never `Herr`. *"It's impossible to this happen"* and after this unit and integration tests were borned to try to track the issue.

No luck! But after complaining with my coworkers, someone suggested that maybe, the database was not case sensitive and it was returning the values that were most found. What do you think? Of course they were right. And one of the ways to do the correct SQL query was:

```ruby
SELECT salutation FROM user WHERE salutation COLLATE utf8_bin NOT IN ('HERR', 'FRAU')
```

What was my mistake again? Assume that this query would be already case sensitive and it wasn't. 

After checking the users with different values, we fixed them and everything worked fine after all. First execution and everything worked like a charm!!!


## what I learnt

- never assume things that you don't know very well
- also when checking the behavior of such small function, ask yourself based on the return if it can behavior differently (case sensitive x case insensitive example)
- have monitoring for important jobs in your system, not just for the main system, but also for background tasks
- the first alarm that was triggered doesn't mean that it was the first problem that happened. Be aware that the first alarm can be a consequence of a non-monitored resource

Do you have stories about debugging? What are your tips to avoid getting stucked in this lovely task that we as developers sometimes need to have fun?

Cheers
