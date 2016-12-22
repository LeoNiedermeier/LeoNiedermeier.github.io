---
title: Method References
tags: [java,java8]
category: java8
summary: "Method References"
---

# Useful Methods

## Set value if not null

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

# References

* <https://docs.oracle.com/javase/tutorial/java/javaOO/methodreferences.html>