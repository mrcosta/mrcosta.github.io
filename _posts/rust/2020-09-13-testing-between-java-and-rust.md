---
layout: post
title: "From @Test to #[test]: an essay about testing between Java and Rust"
date: 2020-09-13 12:00:00 
comments: true
description: ""
categories:
- rust, java, tests, testing, tdd, unit tests, test pyramid
permalink: testing-between-java-and-rust
---
{% raw %}
A disclaimer about used terms in the text: I've seen along the years that some definitions about testing have different meanings to developers. So everything here it's based on my experience along the years: the projects that I was involved, the people that I worked with, blog posts and books that I read, etc. 

# Unit Testing in the projects that I've worked

It was working with Groovy that I have my first contact with [Test Driven Development](https://www.goodreads.com/book/show/387190.Test_Driven_Development), and looking back today, I can say that people in my team was not pragmatic regarding the [Test Pyramid](https://martinfowler.com/articles/practical-test-pyramid.html). We aimed for a high percentage for test coverage in all the pyramid levels. I remember that our concept of [integration tests](https://martinfowler.com/bliki/IntegrationTest.html) was a very specific one.

Basically two objects (two Groovy classes) interacting to each other was considered an integration and, since we were using a more [mockist approach](https://martinfowler.com/articles/mocksArentStubs.html) we used a lot of stubs and mocks. At this point, you can imagine that we had a lot of redundant tests since is impossible to multiples objects to work without some kind of interaction/dependency between them. I think this project was extreme, but since then I carried this mindset with me always trying to test almost everything. 

I was working with Java in my previous company as a consultant and it was a company that was pretty much by the book regarding practices like TDD and other buzzwords in our area as selling points to clients. I remembering convincing one developer to do a very complex unit test that was hard to set up to assert that a log statement was printed. This log statement was being used for our monitoring. And even that my arguments at the time were good, I think we could achieve better things to the business instead of spending almost 3 days to set up something complex to test a cross-functional requirement that could be easily manually tested in a few minutes.

Another case was to test some complex environment configuration to make sure it was there during the startup of the application. Again, my conclusion today is that the time spent trying to test something complex that could be just confirmed starting the application and would give the same confidence than with a complex test would be better to the business. 

Of course, in both cases, we would miss a lot of benefits on the long term like regression and documentation, but even considering this I think it would not be worth doing with a test that required so long time to setup. 

Most of the projects that I worked with Java had this similar mindset that I have described. My mindset had changed in my last project where I worked with a very experienced developer that was more pragmatic with some specific tests and was always reminding me about **how important it is to solve the clients’ problems with quality but being pragmatic when testing some complex things or just forget for a moment about writing several unit tests and invest in at least a few integration tests**. These things I didn't learn in any book. You need to understand your context and have experience with different scenarios, projects, problems, deadlines and people. These details on everyday routine are going to give you enough knowledge to find a good decision that works with the scenario at hand.

# Some differences between Testing in Rust and Java

These stories were told to give an idea of how much I struggled with testing in Rust when I started.

In Java, we need at least one dependency (like `JUnit`) to write unit tests and Rust has support for unit tests in the language by default. I think this is amazing since the possibility of doing tests is just **there** and the syntax is pretty much the same as writing production code. Of course, as your project grows, things get more complex and then you may need some external help from other crates.

Another big difference for me is where you write your unit tests. In Java, you usually have a file for your production code and another one for testing. In Rust, you write them together in the same file.

This was weird for me in the beginning, but it was the beginning! Nothing weird to feel weird if it's something that you're starting to do. Today it makes complete sense since is easier to look at the code and understand what is wrong if some test is falling or some scenario is missing. And of course, they are not compiled in the final binary since rust exclude everything with `#[test]` attribute.

Another thing is the number of scenarios that you need to write. It changes since you don't have `null` like java. `null` in Java can leave you to think in edge cases later, since you can easily get to a situation like *"I don't want to think in error handling now, let's just return `null`"*. And then when running the production code you realize that there is this `null` case that needs to be treated. Rust forces you to think better in the edge cases where things can go wrong since you probably are going to work with types like `Option` and `Result`.

# A practical example

I was implementing a new endpoint in our Rust project at [Impero](https://impero.com/) with a function that looked pretty much like this:

```rust
#[get("/process_query")]
process_query(query_executor: QueryExecutor) -> Result<Processed, InternalError> {
    let something = query_executor.call_to_database_that_return_something();
    let result = complex_private_method_logic_that_I_wanted_to_test(something);
    result
}
```

In Java I would choose between two testing approaches: 
1. an integration test that would hit the endpoint and use an in-memory database
2. a unit test calling the public method and use a `Mock` returning something realistic from the database that I could work with afterwards. 

Some people probably are going to have a strong opinion here about the way to test but I like what [Kent Beck](https://en.wikipedia.org/wiki/Kent_Beck) says in this [podcast](https://pythontesting.net/transcripts/transcript-epsiode-23-lessons-testing-tdd-kent-beck/) about TDD and testing or what kind of specific test you should write in a specific situation:

>I think the level, the interface where you apply your tests, is a pragmatic decision based on the circumstances you're in. I write tests at whatever scale I need them to be to help me make my next step of progress. Part of the skill of TDD is learning to move between scales, right. So I write a test that my customer says, "this scenario should result in a five". So you write a test that says this scenario should result in a five, and then you're down deep in the intestines of your program and you're thinking, oh, I see, well this object when given a five and a seven should return the five. Well, that's a good place to write a test because that's another piece of the story that needs to be told. But, you know, is that Acceptance Test Driven Development, or is that BDD? I think that erecting rigid walls between the styles is actually a mistake, like the scales, as a programmer I want to understand all those scales. Tests help me understand, so I write tests at all those scales.

I tried to apply the number two (with `Mock`) with Rust but there is a problem. There aren't still good mocking crates (you can check a good comparison table [here](https://asomers.github.io/mock_shootout)) for structures. It's being some time since I've faced this issue, but I remember the problem was related to being hard to create mocks for structs in external crates (and the struct to access the database was complex). I remember reading these two posts [here](https://klau.si/blog/mocking-in-rust-with-conditional-compilation/) and [here](https://tech.labs.oliverwyman.com/blog/2018/05/21/on-mocking-rust/), and this good discussion [here](https://www.reddit.com/r/rust/comments/d2puv7/how_to_mock_calls_to_databases/):

> Rust does not have a concept of object inheritance for structs so there is no way to mimic a struct type from the standard library or an external crate.

I hope as I get more fluency with the language and with new or more mature crates I can manage to do this without problems. 

**Another way to see the problem**: maybe my design was broken. And yes this, in fact, can be one of the answers when I compare the way that a connection is passed in `Java` or how the [Dependency Inversion Principle](https://en.wikipedia.org/wiki/Dependency_inversion_principle) works, but this right now seems a question for the future. I'm still exploring the language in other aspects and even that I could say how this is made in other languages, I think I need to see more to have an elaborated opinion.

But back to the testing strategy: **How did I test the code?**. In fact, pairing with my colleague he comes up with a very simple solution: 

>it's just one line of code that is making this database call and it's been already isolated tested in another place, so let's just test the isolated private function and move on. 

That's what we did, and it was hard for me to let it go at the time, but today I understand that was a good decision.

In Java, people would complete forbid this and start screaming about how we only test the public API's/public methods. And I understand it, since is also the way that the language was designed and consequently how the libraries supporting the ecosystem also is going to be affected by that.

I only wanted to highlight that the testing approach here can be different because the two languages are different. Not just by design, but also in how each community can approach practices that are not linked by the language itself. 

Another example: I see the [Single Responsibility Principle](https://en.wikipedia.org/wiki/SOLID) with a different way in Rust than Java. In my experience, in Java, is more common for developers to create smaller classes with small methods while in Rust I see that is more forgivable to have more modules with way more lines that I have seen in Java classes. And this, in general, it's ok I think. The community of either language will figure out later if a different approach needs to be taken.

# Conclusion

When migrating from Java to Rust, I had to revisit a lot of concepts regarding testing. I learn to be way more pragmatic when going too far or spending too much time creating complex setups for tests that in the end would not bring too much value when compared to one specific integration test.

Learning another language also gives you the opportunity to learn new ways of doing things that you can apply in general. Today if I had to code in Java again, I would try to use way more the `Optional` type that I used in the past. After seeing how it works in Rust, is something that I think I could benefit way more in Java today than just using `null`.

Rust is still a young language. I'm sure the testing approach still has a long way to evolve and in a few years, this post probably is going to seem outdated.

My final message is that if you come from another language you probably know that you need to adapt to how the language works (or at least something in between). So you should do the same for the testing approach.



{% endraw %}
