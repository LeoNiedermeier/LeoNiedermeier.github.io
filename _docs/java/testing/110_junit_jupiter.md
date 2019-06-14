---
title: JUnit Jupiter
tags: [testing]
category: testing
summary: "Junit Jupiter"
---
# Common Notes

* Test classes and methods need not to be public but can be package private.
  * Reduces unwanted visibility of classes and methods, useful e.g. in Eclipse.
* Can have nested test classes.
  * Helps to structure test classes.

# Grouping Tests

Grouping of test classes:

* Per class (<http://xunitpatterns.com/Testcase%20Class%20per%20Class.html>)
  * Most common approach
  * Can lead to large test classes
  * Test class naming: <class to test> + "Test"
* Per feature (e.g method) (<http://xunitpatterns.com/Testcase%20Class%20per%20Feature.html>)
  * Can lead to many fine grained test classes
  * Example test class naming for Test class for method: <class to test> +"_" + <method to test> +"_" + "Test" (The "_" separates 
  the class and method part of the name. Without a delimiter it will be unclear, what is part of the class name and what is 
  part of the method name)
* Per fixture setup (<http://xunitpatterns.com/Testcase%20Class%20per%20Fixture.html>)


## JUnit Jupiter `@Nested` annotation
See <https://junit.org/junit5/docs/current/api/org/junit/jupiter/api/Nested.html>

* Group test methods for feature / methods
* Can share state and initialization with outer class
* Grouping of test classes can be
  * Test class per class
  * Nested test class per method / feature

A nice example is <https://github.com/apache/commons-lang/blob/master/src/test/java/org/apache/commons/lang3/ValidateTest.java>


# Assertions

See <https://junit.org/junit5/docs/current/user-guide/#writing-tests-assertions>

* Methods of class `org.junit.jupiter.api.Assertions`

Can also use Hamcrest <http://hamcrest.org/JavaHamcrest/index> with the `org.hamcrest.MatcherAssert.assertThat(...)` methods.
Background:
<https://objectpartners.com/2013/09/18/the-benefits-of-using-assertthat-over-other-assert-methods-in-unit-tests/>


# References
* <https://junit.org/junit5/>
* <http://xunitpatterns.com>
* <https://github.com/jupiter-tools>