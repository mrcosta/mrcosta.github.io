---
layout: post
title: "How to Override Jersey's Jackson Exceptions"
date: 2017-07-03 12:00:00 
comments: true
description: " These days I was working with Jetty (*version 2.19*) to build a Restful API and it's come in hand with Jackson. What it means that some basic JSON validations will happen automagically. Nice, right? Hum...Let'see."
categories:
- java, api, rest, parser
permalink: 2017-07-03-override-jerseys-jackson-validation-exception
---
https://twitter.com/AndreaPessino/status/1042120431942029312

- Try to STOP thinking in terms of the patterns you are used to – do not try to replicate object oriented idioms with Rust. None of these abstractions are necessary or especially beneficial, and while they
have their place the early exploration of a language that does not specifically support them is not it. Start (or go back to) thinking in terms of data layout and interface design, instead of reducing problems to fixed patterns. 

https://klausi.github.io/rustnish/2019/03/31/mocking-in-rust-with-conditional-compilation.html

https://tech.labs.oliverwyman.com/blog/2018/05/21/on-mocking-rust/

https://blog.merigoux.ovh/en/2019/04/16/verifying-rust.html

One way to increase confidence is to test the program in more systematic ways, for instance by doing unit testing or measuring the code coverage and making sure it’s 100%. Let us say that we are testing the function:

fn foo(x: i32, y:i32) -> i32 {
  ...
}
Unit testing means writing tests for every sub-function used in foo. Having a code coverage of 100% on foo means that your test case explore all the branches in the code of foo: if foo contains a test x + y > 1, it means we should have at least one test case where x + y > 1, and one where x + y <= 1. But even a 100% code coverage does not guarantee that your tests reach all the program states that could cover bugs; overflow bugs are not always detected by such testing for example.

Testing a program in such a systematic way is time-consuming, and requires the programmer to create a mental model on how the code should work, in order to create test cases as relevant as possible. When all the tests are written with this methodology, there are two correlated outcomes:

The programmer has thought about the behavior of its code for all combinations of the input variables;
The level of assurance about the correctness of the code is high enough for a production release.
However, these methods cannot tell you if you missed something; even if you carefully test your code to prevent a certain category of bug, there will always be a possibility that a bug of this category remains hidden somewhere.

One way to increase confidence is to test the program in more systematic ways, for instance by doing unit testing or measuring the code coverage and making sure it’s 100%. Let us say that we are testing the function:

fn foo(x: i32, y:i32) -> i32 {
  ...
}
Unit testing means writing tests for every sub-function used in foo. Having a code coverage of 100% on foo means that your test case explore all the branches in the code of foo: if foo contains a test x + y > 1, it means we should have at least one test case where x + y > 1, and one where x + y <= 1. But even a 100% code coverage does not guarantee that your tests reach all the program states that could cover bugs; overflow bugs are not always detected by such testing for example.

Testing a program in such a systematic way is time-consuming, and requires the programmer to create a mental model on how the code should work, in order to create test cases as relevant as possible. When all the tests are written with this methodology, there are two correlated outcomes:

The programmer has thought about the behavior of its code for all combinations of the input variables;
The level of assurance about the correctness of the code is high enough for a production release.
However, these methods cannot tell you if you missed something; even if you carefully test your code to prevent a certain category of bug, there will always be a possibility that a bug of this category remains hidden somewhere.
