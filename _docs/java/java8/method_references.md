---
title: Method References
tags: [java,java8]
category: java8
summary: "Method References"
---

# Useful Methods

## Set Value if Not Null

Example: set a value in a bean if the value is not null.

~~~ java
static <T> void setIfNotNull(T value, Consumer<T> target){
  if(value != null){
    target.accept(value);
    }
}
~~~

Usage

~~~ java
Target target = ...;
// Target has a method setSting(String)

setIfNotNull("myValue", target::setString);

~~~

## Comparator With Method References

With the `Comparator.comparing` and `Comparator.thenComparing` it is easy to create `Comparator`s. 

From <https://docs.oracle.com/javase/8/docs/api/java/util/Comparator.html#comparing-java.util.function.Function->

> Accepts a function that extracts a Comparable sort key from a type T, and returns a Comparator<T> that compares by that sort key.

From <https://docs.oracle.com/javase/8/docs/api/java/util/Comparator.html#thenComparing-java.util.function.Function->
> Returns a lexicographic-order comparator with a function that extracts a Comparable sort key.

{: .info title="Note"}
The key extractor og this methods has a return type which implements `Comparable`

Example:

~~~ java
Comparator.comparing(Person::getLastName).thenComparing(Person::getFirstName)
~~~

Compares the persons by their last name and first name. 


# References

* <https://docs.oracle.com/javase/tutorial/java/javaOO/methodreferences.html>