---
title: Method References
tags: [java,java8]
category: java8
summary: "Method References"
---

# Examples

~~~java

// object::instanceMethod
Function<String, Integer> f = String::length;
// or
ToIntFunction<String> f = String:: length;
// same as
ToIntFunction<String> f = s -> s.length();

// class::instanceMethod
BiFunction<String, String, String> f = String::concat;
// same as
	BiFunction<String, String, String> f = (a,b) -> a.concat(b);

~~~

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
The key extractor of this methods has a return type which implements `Comparable`

Example:

~~~ java
Comparator.comparing(Person::getLastName).thenComparing(Person::getFirstName)
~~~

Compares the persons by their last name and first name. 


## Interface With Multiple Methods

An interface with multiple methods is not a functional interface. Therefore, it is not a one liner to use method refereces or lamdas for it. Consider the exampe interface

~~~java
interface MyInterface {
  void foo(String s);
  String bar(String p1, String p2);
}
~~~

With a simple factory method and an adequate implementation, we can connect it to method references:

~~~java
static MyInterface of(Consumer<String> foo, BiFunction<String, String, String> bar) {
  return new MyInterface() {
    public void foo(String s) {
      foo.accept(s);
    }
    public String bar(String p1, String p2) {
      return bar.apply(p1, p2);
    }
  };
}
~~~

~~~java
MyInterface myInterface = of(System.out::println, "a_a"::replaceAll);
~~~

Same can be done for classes, for instance for `MouseListener``:
~~~java
import java.awt.event.MouseAdapter;
import java.awt.event.MouseEvent;
import java.awt.event.MouseListener;
import java.util.function.Consumer;
...
MouseListener forMouseClicked(Consumer<MouseEvent> mouseClickListener) {
  return new MouseAdapter() {
    public void mouseClicked(MouseEvent e) {
      mouseClickListener.accept(e);
    }
  };
}
~~~

# References

* <https://docs.oracle.com/javase/tutorial/java/javaOO/methodreferences.html>
