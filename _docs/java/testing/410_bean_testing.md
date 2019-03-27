---
title: Bean Testing
tags: [testing]
category: testing
summary: "Testing pf bean properies"
---

# Overview

Referenced classes are part of the project `io.github.leoniedermeier.utils`.

# Individual Getters and Setters
The class `io.github.leoniedermeier.utils.test.beans.BeanTesterUtils` provides methods to test that bean getter returns the 
same object which is set by the setter.

Example:
~~~java
 BeanSetterGetterTester.assertSetterGetter(MyBean.class, "My-Input-Value", MyBean::setName, MyBean::getName);

// or with random input value
 BeanSetterGetterTester.assertSetterGetter(MyBean.class, MyBean::setName, MyBean::getName);
~~~

{ .code .x title="Example Failure Trace"}
~~~
org.opentest4j.AssertionFailedError: Reason: Setter - Getter different values!
set-method: setWrong
get-method: getWrong
 ==> expected: <b874e5df-62e4-44be-857b-229212c811b7> but was: <WRONG: b874e5df-62e4-44be-857b-229212c811b7>
~~~

# Generic Getter and Setter Testing

The class `io.github.leoniedermeier.utils.test.beans.GenericBeanPropertyTester` provides a method to test all getters and setters 
(non recursive!). The bean properties are determined by `java.beans.Introspector.getBeanInfo(...)`.

For all bean properties the following steps are executed:

* set a random value by the setter
* call the getter
* check the values

Tested bean properties can be of type:

* primitives or their wrapper classes
* `String`
* any class which have a default constructor (instances are created by `java.lang.Class.newInstance()`)

Example:
~~~java
GenericBeanPropertyTester.testAllSettersGetters(MyBean.class);
~~~

{ .code .x title="Example Failure Trace"}
~~~
org.opentest4j.MultipleFailuresError: Test properties getter and setters (2 failures)
    -----------------
property: number
set-method: setNumber
get-method: getNumber
 ==> expected: <-1459853287> but was: <12>
    -----------------
property: wrong
set-method: setWrong
get-method: getWrong
 ==> expected: <27e11e18-436f-4aca-b9dd-fe94bfd97f57> but was: <WRONG>
~~~
