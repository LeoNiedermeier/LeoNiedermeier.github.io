---
title: Java 8 Streams
tags: [java,java8]
category: java8
summary: "Java 8 Streams"
---

# Useful Methods

## Filters

## Filter nulls

~~~ java

...filter(Objects::nonNull);
~~~

### Filter Elements of Given Type

A combination of `Class.isInstance` and `Class.cast` together with `Stream.filter` and `Stream.map` results in a stream of objects with the desired type:

~~~ java
Stream.of("String", Integer.valueOf(1))
// reduce to a stream of Integer:
.filter(Integer.class::isInstance).map(Integer.class::cast)
~~~


# References