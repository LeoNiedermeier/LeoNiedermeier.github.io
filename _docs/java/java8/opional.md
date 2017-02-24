---
title: Java 8 Optional
tags: [java,java8]
category: java8
summary: "Java 8 Optional class and how to use"
---

# Overview

> But we did have a clear intention when adding this feature, and it was not to be a general purpose Maybe or Some type, as much as many people would have liked us to do so. Our intention was to provide a limited mechanism for library method return types where there needed to be a clear way to represent "no result", and using null for such was overwhelmingly likely to cause errors.  
(<http://stackoverflow.com/questions/26327957/should-java-8-getters-return-optional-type/26328555#26328555>)

{: .info title="Not Serializable"}
`Optioal` does not implement `Serializable`. 
(see also <http://stackoverflow.com/questions/24547673/why-java-util-optional-is-not-serializable-how-to-serialize-the-object-with-suc>)

Other interesting things can be found at <http://blog.codefx.org/java/dev/design-optional/>

# Usage Scenarios

## Simplify Nested Bean Access

Accessing a nested bean can be of the form:

~~~java
Person person = ...
String street = person.getAddress().getStreet();
~~~

The access to the nested beans and properties has to be validated against `null`. This leads to verbose code:
 
~~~java
Person person = ...
String street;
if(person != null && person.getAddress() != null){
  street = person.getAddress().getStreet();
 }
~~~

With the help of `java.util.Optional` this can be written like

~~~
Person person = ...
String street = Optional.ofNullable(person).map(Person::getAddress).map(Address::getStreet).orElse("Default Street");
~~~

The resulting code is shorter and the probability of making a mistake is substantially reduced.

{: .info title="Helper Class"}
In this context the `java.util.Optional` plays a role of a bean accessor helper class. The naming for helper class could be like `PropertyAccessor.with(person).then(Person::getAddress).then(Address::getStreet).orElse("Default Street")`. 

# References

* <http://www.oracle.com/technetwork/articles/java/java8-optional-2175753.html>
* <http://stackoverflow.com/questions/23454952/uses-for-optional>
* <https://docs.oracle.com/javase/8/docs/api/java/util/Optional.html>
* <http://stackoverflow.com/questions/26327957/should-java-8-getters-return-optional-type/26328555#26328555>
* <http://blog.codefx.org/java/dev/design-optional/>