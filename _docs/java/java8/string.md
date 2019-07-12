---
title: Strings etc
tags: [java,java8]
category: java8
summary: Strings etc.
---

# String Concatination

* Take readability into account
* The compiler does some optimizations
    * depends on java version
    * hand crafted optimizations can not be optimized by the compiler later

* Whenever possible use `+`
* Note that an assignement creates a new string instance
    * do not use s = s + "someString" or similar in a loop

## Pre Java 9
~~~java
String m(String a, int b) {
  return a + "(" + b + ")";
}
~~~

The compiler generates somthing like
~~~java
String m(String a, int b) {
  return new StringBuilder().append(a).append("(").append(b).append(")").toString();
}
~~~

# References

* <https://github.com/kabutz/string-performance/blob/master/Enough%20java.lang.String%20to%20Hang%20Ourselves%20....pdf>
* <http://openjdk.java.net/jeps/280>
