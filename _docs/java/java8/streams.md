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

# Exceptions

Exceptions which are thrown due to intermediate operations occur when the stream is processed.

**Example** 

~~~java

public void test(){
		integerStream().forEach(System.out::println);
	}

	private Stream<Integer> integerStream() {
		try{
			return Stream.of("1","noNumber","3").map(Integer::valueOf);
		}
		catch (Exception e) {
			e.printStackTrace();
		}
		return Stream.empty();
	}
~~~

In this case the `NumberFormatException` which is thrown from `Integer::valueOf` due to parse errors of "noNumber" is **not** catched from the catch block in the `integerStream` method. The Exception is thrown from the `integerStream` method.

<div class="danger" title="Returning Stream from DAO method with @Transactional annotation." markdown="1">

Consider a typical spring application with a DAO which has a method like following: 

~~~java
@Transactional
Stream<Person> getAll(){
	return ...
}
~~~

Note that the method is annotated with `@Transactional`. In the case that the returned stream throws an exception during processing, the `@Transactional` has **no** effect. Because the stream processing is executed outside the `@Transactional` context (not processed in the `getAll` method).

</div>

{: .success title="Tip"}
Do not return streams which have (maybe hidden) processing dependencies to the context.


# References

* <http://stackoverflow.com/questions/24676877/should-i-return-a-collection-or-a-stream>