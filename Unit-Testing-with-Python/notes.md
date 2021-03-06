# Course Summary

- Unit testing vocabulary & basic example using `unittest`
- Unit test - why and when
- An alternative to `unittest`: `pytest`
- Testable documentation using `doctest`
- Test doubles
- Assessing test suite coverage
- Maintainable unit tests


# Module Summary

- Unit testing vocabulary & "Phoneback" example
    - test suite
    - test case
    - test runner
- Test case design


# Review of Fundamentals

### System Under Test

- a unit test checks the behavior of an **element of code**
    - a method or function
    - a module or class

- an automated test
    - designed by a human
    - runs without intervention
    - report results unambiguously as "pass" or "fail"

- it's not a unit test if it uses
    - the file system
    - a database
    - the network


# Exercise - Phone Numbers

- Given a list of names and phone numbers, make a Phonebook that allows you to look up numbers by name

- Determine if a given Phonebook is consistent
    - In a consistent phone list, no number is a prefix of another
        - `Bob          91125436`
        - `Alice        97 625 922`
        - `Emergency    911
    - Bob and Emergency are inconsistent!


# Vocabulary

### Test Case

- each one is for a specific behavior of system
- each should be run independently of others
- clearly report whether it has passed or failed
- should not have side effects other test cases will rely on
    - i.e. a test case shouldn't create data another test case will use

### Test Runner

- executes test cases and reports the results
- in the `unittest` module, the command line test runner is built-in
- shouldn't be considered when writing test cases
 

### Test Suite

- a number of test cases that are executed together by a test runner


### Test Fixture

- a piece of code that can construct and configure the system of the test to get it ready to be tested, and then clean up afterwards
- allows separation of concerns so the test cases can concentrate on specifying and checking a particular behavior, and not be cluttered by general set-up details
- `setUp` and `tearDown`
- in pure unit testing when all objects are in memory, there is usually no explicit need for `tearDown` step because the memory manager would take care of the cleanup


### Test Case Design

- **Arrange**: set up the object to be tested & collaborations
- **Act**: exercise functionality on the object
- **Assert**: make claims about the objects & its collaborators
- **Cleanup**: release resources, restore to original state


    
    
***


# What is Unit Testing For?

![](./images/test-suite-diagram.png)

## Understanding

- Collaborate with people in other roles to understand what's needed
    - Business Analyst, Product Owner
    - Tester
    - Interaction Designer
    - Lead Developer

## Documenting

- "Executable Specification"
    - Tests document the behavior of the code
    - How the unit is intended to be used

## Design

- Decompose the problem into units that are independently testable
    - loose coupling
- Design the interface separately from doing the implementation

## Regression Protection

- **Regression** - something worked before and doesn't anymore
    - A unit test should fail and point out which unit failed and why


# Limitations of Unit Testing

- Cannot guarantee that software is 100% bug-free
- Testing can't find all the errors and prove correctness
- Unit testing won't find integration errors


# Personal Development Process

## Test First

![](./images/test-first-diagram.png)

- Design Code $\rightarrow$ Design Tests $\rightarrow$ Write Code
    - Forces to design a testable interface before investment in implementation
    - Helps cover the important behaviors with test cases
- Risk: Rework

## Test Last

![](./images/test-last-diagram.png)

- Code first, design tests later
- Build automated regression tests
- Risk: discover testability problems and bugs late in the processfo
    - Example: Not suitable for `TelemetryDiagnosticControls` because of lack of testability (had to change constructor function)
- Risk: you'll rush or skip designing the tests
- Design Code $\rightarrow$ Design Tests $\rightarrow$ Debug & Rework $\rightarrow$ Design Code $\rightarrow$ Cycle Continues ...

## Test Driven

![](./images/test-driven-diagram.png)


# Unit Testing in the Wider Development Process

<http://www.martinfowler.com/articles/continuousIntegration.html>


***


# xUnit

- JUnit was originally created by Kent Beck and Erich Gamma

# Test Fixture Functions

![](./images/test-fixture-functions.png)

- Create test fixture functions that can provide resources to test cases
- Test cases will request a resource they need (sort of a dependency injection)

```python
@pytest.fixture
def resource:
    return Resource()
```

- *Unit tests should be in memory*


***

- Situations when you'd use `doctest`
- Making documentation comments more truthful
- Handling output that changes
- Using `doctest` for regression testing

# [`doctest`](docs.python.org/3.3/library/doctest.html)

- Checking examples in docstrings
- Regression testing
- Tutorial documentation

```python
def factorial(n):
    """Return the factorial of n, an exact integer >= 0

    >>> [factorial(n) for n in range(6)]
    [1, 1, 2, 6, 24, 120]
    >>> factorial(30)

    >>> factorial(-1)
    Traceback (most recent call last):
        ...
    ValueError: n must be >= 0

    Factorials of floats are OK, but the float must be an exact integer:
    >>> factorial(30.1)
    Traceback (most recent call last):
        ...
    """
```


# Handling Output That Changes

- Dictionaries
- Floating point numbers
- Object ids
- Tracebacks?

# `doctest` Directives

`#doctest: +ELLIPSIS`

- Directives control how doctest matches output
- Use wildcard matching with care

<http://docs.python.org/3.3/library/doctest.html#doctest-directives>


# Approval Testing

- "I'll know it when I see it"

![](./images/approval-testing.png)


***


# [Test Double](https://en.wikipedia.org/wiki/Test_Double)

![](./images/test-double.png)

- like "Stunt Doubles" who stand in for actors in films
- class under test doesn't know it isn't talking to the real object
- Allow you to control what happens to your class under test

## Stub

- Returns a hard coded answer to any query
- Stands in for collborating objects that is difficult to use in a test case
- Same interface
- But has no logic or advanced behavior


## Fake

A real implementation, yet unsuitable for production.

Common things to replace with fakes:

![](./images/test-fakes-replacement.png)


## Mock

A stub that additionally verifies interactions.

### Three Kinds of Assertions

1. Check the return value or an exception
2. Check a state change (use a public API)
3. Check a method call (use a mock or spy)


## Spy

Lets you query afterwards to find out what happened.


## Dummy Object

For when the interface requires an argument.


# Test Isolation

![](./images/test-isolation.png)


***


# Monkeypatching

- Changing code at runtime


***


# Interpreting Coverage Data

- Find missing test cases
- Get legacy code under test
- Continuous integration - constant measurement


# Test Quality != Coverage

![](./images/test-quality-coverage.png)