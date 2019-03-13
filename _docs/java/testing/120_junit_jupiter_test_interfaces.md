---
title: JUnit Jupiter Test Interfaces
tags: [testing]
category: testing
summary: "Using default methods of interfaces as test methods."
---

# Overview

* Provide reusable test methods
* A class can implement multiple test interfaces
  

# Examples

Tests the contracts of comparable and equals:
* https://github.com/junit-team/junit5/tree/master/documentation/src/test/java/example/defaultmethods

## Check the Util Class Contract

Util classes should
* be public
* be final  
* have one private constructor
    

{: .code .x title="UtilClassConventionsTester"}
~~~java
public interface UtilClassConventionsTester {

    Class<?> getClassUnderTest();

    @Test
    default void hasOnlyOnePrivateConstructor() {
        Class<?> clazz = getClassUnderTest();
        final Constructor<?>[] cons = clazz.getDeclaredConstructors();
        assertEquals(1, cons.length, "Class " + clazz + "has more than one constuctor.");
        assertTrue(Modifier.isPrivate(cons[0].getModifiers()), "Constuctor of class " + clazz + "is not private.");
    }

    @Test
    default void isPublicClass() {
        Class<?> clazz = getClassUnderTest();
        assertTrue(Modifier.isPublic(clazz.getModifiers()), "Class " + clazz + " is not public.");
    }

    @Test
    default void isFinalClass() {
        Class<?> clazz = getClassUnderTest();
        assertTrue(Modifier.isFinal(clazz.getModifiers()), "Class " + clazz + " is not final.");
    }
}
~~~

Usage

{:code .x title ="Usage of UtilClassConventionsTester"}
~~~ java
    // as Nested Test:
    @Nested
    class MyUtilsUtilClassConventionsTest implements UtilClassConventionsTester<MyUtils> {
        public Class<?> getClassUnderTest() {
            return MyUtils.class;
        }
    }
~~~


# References

* <https://junit.org/junit5/docs/current/user-guide/#writing-tests-test-interfaces-and-default-methods>
* <https://github.com/junit-team/junit5/tree/master/documentation/src/test/java/example/defaultmethods>  