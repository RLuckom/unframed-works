### The Sandwich Test

tl;dr: If you have above 90% code coverage, you probably have stupid unit tests.

I don't practice TDD, but when I get too far ahead of my tests I find it impossible
to move forward until I go back and write them. This is because I am lazy. I will not
perform a multi-step process manually just to find out that my code has bugs in it. I
will automate that, tyvm. And I am unwilling to assume that my code works without the
multi-step process being run, because I'm neither an idiot nor a megalomaniac. Without
good tests programming is just trumped-up theology.

But many tests are dumb. I love picking on front-end unit tests, because they frequently
include assertions that {correct http request} is sent. No one cares about that. People
writing those tests usually only care either about the data that is displayed on the
screen (GET) or the state of the data in the persistence layer (POST|PUT|DELETE). 
The fact of the http request is an implementation detail that should not be tested.
If you're testing how data is displayed on the screen, use a mock to supply that data
to the display code (or better, don't; just test the whole system). The display code
should be far enough removed from the http request code that it's easy to test in
isolation. If you're trying to be assured of the state of the data in the persistence layer,
test with the actual persistence layer.

Good tests have some properties:

  1. Good tests exercise complete vertical pieces of functionality. In general, asserting that functions
     are called with specific arguments, or that code tries to make network calls, creates
     untested boundaries between the tests at exactly the interface that is most likely to change.
     I'm not sure if these tests originated in spec-driven development (because they *are*
     appropriate for testing software that is supposed to implement a spec exactly), but they
     are not well-suited to codebases where both sides of an interface are subject to change,
     such as nearly all non-library code. Good tests do not have the vestigial stumps of one-half
     of an interface; they either test across it, such as using both a client and a server, or
     they stay strictly on one side of it, such as testing the business logic behind an API.
  2. Good tests adhere to the principle of Laziness in All Things. That means that they test the
     maximum amount of product functionality per unit of test complexity.
  3. Good tests exercise the desired product functionality as generally as possible. Test cases 
     include a wide variety of inputs and outputs that could be encountered in the life of the code,
     rather than the specific set of inputs and outputs that the developer knows will
     exercise the existing code paths.
  4. In failure, good tests point to specific, fixable problems in the code.
  5. Good tests are simple to extend. The first tests around a piece of functionality in the
     product should establish an efficient idiom for testing that piece of functionality.
     This idiom should allow adding new tests easily, and should also allow generating test cases.
  6. Good tests are easy to maintain. When you establish an idiom for testing a section of
     functionality in the product, it should encapsulate the setup, testing, and teardown
     in a reusable way, so that small-to-medium changes in the code don't require rewriting
     the tests.

Bad tests have the opposite properties:

  1. Bad tests leave untested boundaries at interfaces. Two microservices are supposed to talk to
     each other, but their tests use mocks. One microservice changes, and changes its tests.
     Both sets of tests pass. When deployed together, they break. In a competent organization,
     this situation will still be caught, because there will also be integration tests that use
     both services at once. But that just highlights the fact that the tests with mocks are
     pointless--it would be better to throw out the mocks, test the libraries behind the interface,
     and leave the interface entirely to the integration tests.
  2. Bad tests spend any amount of complexity to meet arbitrary code coverage goals.
  3. Bad tests follow the structure of the implemented code. They test specifically that none of the
     bad things that the developer can think of at the moment will cause an error, or that they
     will cause the right error.
  4. Failures of bad tests are due to correct behavior the test author didn't anticipate more often than
     to actual bugs in the product code. Bad tests are susceptible to failure because of small timing
     differences in execution environments, but the failures don't provide clear error messages
     and the tests are ambiguous about the expected behavior (possibly because the expected
     behavior is 'execute the `false` code path through the third `if` statement'), so it's more
     tempting to run them again and hope for the best than to try to figure out what's wrong.
  5. Bad tests are hard to extend when new bugs are found. Writing the fifth test case for a piece
     of functionality is as hard as writing the first.
  6. Bad tests are difficult to maintain in the face of small changes in the product code. When the
     front-end code is changed to make a new GET call on the home page, ALL the front-end tests
     have to change because they need the new mock.

My most basic principle of testing is something I think of as the sandwich test. Pretend someone took away
your microservice, or some other important part of your stack, and replaced it with a sandwich. Then
imagine that this sandwich actually behaved a lot like your microservice. Imagine that it provided 
an identical API to that of your microservice. Imagine that even though your microservice had
been replaced with a sandwich, all of your tests pass. If your tests are really good tests, and they
actually test the things you care about in your application, you should be happy to deploy that
sandwich in production. Hell, if you use continuous deployment, you might be running a sandwich in
production right now (and if you don't, you're almost certainly running a sandwich, except as soon as it was
deployed someone had to log on to the sandwich to add ketchup and mustard, and the bread has probably
gone stale).
