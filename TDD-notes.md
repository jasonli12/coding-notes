# Test Driven Development (TDD)

**Why We Write Test Code**
+ To ensure that production code does what it's intended to do.
+ To ensure production code doesn't break when you refactor it. Refactoring is the process of restructuring existing computer code - changing the factoring - without changing its external behavior.

**Test Driven Development (TDD)**: an approach to development where you write a test before you write just enough production code to fulfill that test and refactoring. This means that every line of code ends up being tested.

1. Add a test.
2. Run the tests.

  1. All tests pass, continue development, add a new test and repeat cycle.
  2. Any test fails, make a small change and run tests again.

    1. All tests pass, continue development, add a new test and repeat cycle.
    2. Any test fails, make another small change and run tests again.

Write your test code before your functional code in very small tests - one test and a small bit of corresponding functional code at a time.

Programmers taking a TDD approach will not write a new function until a test fails because that function does not exist. In other words, no code will be added unless a test requires it to be added.

**Two Levels of TDD**

+ Acceptance TDD (ATDD)/Behavioral Driven Development (BDD): you write a single acceptance test (business requirements, features, etc) and then just enough production code to fulfill that test. Some tools include RSpec and Fitnesse.

+ Developer TDD: you write a single developer test and then just enough production code to fulfill that test. Some tools include JUnit and VBUnit.

Combining Acceptance TDD and Developer TDD

1. Add an acceptance test.
2. Run the acceptance tests.
  1. All acceptance tests pass, add the next acceptance test.
  2. Any acceptance test fails, make a small change and add a developer test.
    1. Run the developer tests.
      1. All developer tests pass, rerun acceptance tests.
        1. All acceptance tests pass, add the next acceptance test.
        2. Any acceptance test fails, make a small change, add a developer test and repeat cycle.
      2. Any developer test fails, make a small change, rerun developer tests and repeat cycle until all developer tests pass.

Effective tests:
+ Run fast (i.e. have short setups, run times and break downs).
+ Run in isolation.
+ Use data that makes them east to read and understand.
+ Use real data when needed.
+ Represent one step towards your overall goal.

**Red-Green-Refactor-Cycle**:

1. *Think*: Figure out what test will best move your code to completion.
2. *Red*: Write a very small amount of code, run the tests and watch the new test fail. The test bar should turn red now.
3. *Green*: Write a very small amount of production code, run the tests and watch the new tests pass. The test bar should turn green now.
4. *Refactor*: Look at the code you've written and ask how you can improve it (ex. duplication, 'code smells'). After refactoring, rerun the tests and make sure they still pass.
5. *Repeat*: Do it again.
