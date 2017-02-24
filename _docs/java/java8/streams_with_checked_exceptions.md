---
title: Java 8 Streams and Checked Exceptions
tags: [java,java8]
category: java8
summary: "Java 8 Streams and checked exceptions"
---

# Overview

If a method which is used within a Java 8 stream processing throws a checked exception, this exception has to be handled. One way to do this is to wrap the checked exception with a `java.lang.RuntimeException` and throw it. This results in a extended stacktrace. 

# Alternative Solution

{: .info title="Note"}
This is just a proof of concept inspired by the  `org.apache.commons.lang3.exception.ExceptionUtils.rethrow(Throwable)` method.

## Background

The `org.apache.commons.lang3.exception.ExceptionUtils.rethrow(Throwable)` method has an interesting functionality:

>Throw a checked exception without adding the exception to the throws clause of the calling method. This method prevents throws clause pollution and reduces the clutter of "Caused by" exceptions in the stacktrace.  
...  
(see <https://commons.apache.org/proper/commons-lang/javadocs/api-release/org/apache/commons/lang3/exception/ExceptionUtils.html#rethrow-java.lang.Throwable->)


## Implementation

First define a functional interface similar to `java.util.function.Function<T, R>` which throws a checked exception:

~~~java
@FunctionalInterface
interface FunctionWithCheckeException<T, R> {
    R apply(T t) throws Exception;
}
~~~

Then we define a small helper method which "transforms" the  `FunctionWithCheckeException` to a `Function`. This method catches also the checked exceptions and rethrows them: 

~~~java
static <T, R> Function<T, R> toUnchecked(FunctionWithCheckeException<T, R> f) {
    return t -> {
        try {
            return f.apply(t);
        } catch (Exception e) {
            throwWithTypeErasure(e); // propagates a checked exception
            throw new IllegalStateException(); // needed for compiler
        }
    };
}

private static <T extends Throwable> void throwWithTypeErasure(Throwable throwable) throws T {
    throw (T) throwable;
}
~~~

The `throwWithTypeErasure` method is similar to `org.apache.commons.lang3.exception.ExceptionUtils.rethrow(Throwable)`. 

## Example

~~~java

public static void main(String[] args) {
    Stream.of("1", "2", "3")
        .map(toUnchecked(Test::functionWithChecked))
        .forEach(System.out::println);
}

int functionWithChecked(String s) throws ClassNotFoundException {
    throw new ClassNotFoundException();
}
~~~

This prints the stacktrace;

~~~
Exception in thread "main" java.lang.ClassNotFoundException
	at Test.functionWithChecked(Test.java:44)
	at Test.lambda$0(Test.java:54)
	at java.util.stream.ReferencePipeline$3$1.accept(ReferencePipeline.java:193)
	at java.util.Spliterators$ArraySpliterator.forEachRemaining(Spliterators.java:948)
	at java.util.stream.AbstractPipeline.copyInto(AbstractPipeline.java:481)
	at java.util.stream.AbstractPipeline.wrapAndCopyInto(AbstractPipeline.java:471)
	at java.util.stream.ForEachOps$ForEachOp.evaluateSequential(ForEachOps.java:151)
	at java.util.stream.ForEachOps$ForEachOp$OfRef.evaluateSequential(ForEachOps.java:174)
	at java.util.stream.AbstractPipeline.evaluate(AbstractPipeline.java:234)
	at java.util.stream.ReferencePipeline.forEach(ReferencePipeline.java:418)
	at Test.main(Test.java:15)
~~~

Note that there is the original exception on the top of the stacktrace.

# References
* <https://commons.apache.org/proper/commons-lang/javadocs/api-release/org/apache/commons/lang3/exception/ExceptionUtils.html>