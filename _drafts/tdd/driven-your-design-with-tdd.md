How hard the function is to use (number of parameters, understanding of output by function name)
Potential risks for the function living in the wild and being used by other developers
Whether the function is doing too much, either because you have to mock the world for it to even run, or if you are asserting too many things per function

I’ve heard people complain that fixing broken tests is hard, time consuming and/or a waste of time. My response is where would you rather be fixing that bug? Would you rather it be in production while people are angry features are broken, or in a unit test, prolonging the time it takes to complete a task?

and writing the unit test will make you think about the design and it needs to do. it will be easier to think if is necessary to check for nulls or not. if is necessary to create a default value for returning. Things that maybe you will just remember after the code is in production. Creating the tests for each of this scenarios mhttp://www.hamvocke.com/blog/testing-java-microservices/ake you life easier


To me, it seems as if we could move the bug fixing earlier in the process and spend more time focussing in code clarity which promotes understanding of the code. Half of the time spent fixing a bug is figuring out how the hell it happened. If you had a unit test it would tell you as soon as you change something and run the test.


At the same time they shouldn’t be tied to your implementation too closely.

Why’s that?

Tests that are too close to the production code quickly become annoying. As soon as you refactor your production code (quick recap: refactoring means changing the internal structure of your code without changing the externally visible behavior) your unit tests will break.

This way you lose one big benefit of unit tests: acting as a safety net for code changes. You rather become fed up with those stupid tests failing every time you refactor, causing more work than being helpful and whose idea was this stupid testing stuff anyways?

What do you do instead? Don’t reflect your internal code structure within your unit tests. Test for observable behavior instead. Think about

“if I enter values x and y, will the result be z?”
instead of

“if I enter x and y, will the method call class A first, then call class B and then return the result of class A plus the result of class B?”
Private methods should generally be considered an implementation detail that’s why you shouldn’t even have the urge to test them.
