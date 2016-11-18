---
title: Validate
tags:
  - java
  - apache_commons
category: apache_commons  
summary: Some example usages of the org.apache.commons.lang3.Validate class.
---
# Overview

>**From org.apache.commons.lang3.Validate javadoc:**
>
> This class assists in validating arguments. The validation methods are based along the following principles:    
> * An invalid null argument causes a `java.lang.NullPointerException`.
> * A non-null argument causes an `java.lang.IllegalArgumentException`.
> * An invalid index into an array/collection/map/string causes an `java.lang.IndexOutOfBoundsException`.


Since Java 7 the `java.util.Objects` provides some validation methods which checks for null. The `org.apache.commons.lang3.Validate` provides
much more validation methods and the ability to construct (validation error) messages with parameters.  

# Some Methods

## Validate not null

```java
Some(final Object argument) {
  Validate.notNull(argument, "Argument must not be null!");
  this.prop = argument;
  ...
```
```java
java.lang.NullPointerException: Argument must not be null!
	at org.apache.commons.lang3.Validate.notNull(Validate.java:222)
    ...
```		

### java.util.Objects alternative
```java
Some(final Object argument) {
  this.prop = Objects.requireNonNull(argument,"Argument must not be null!");
  ...
```

```java
java.lang.NullPointerException: Argument must not be null!
	at java.util.Objects.requireNonNull(Objects.java:228)
    ...
```		

## Validate not empty
With the various `notEmpty` methods one can check
* array
* `java.util.Collection`
* `java.util.Map`
* `java.lang.CharSequence`

for emptiness. If the argument is null, a `java.lang.NullPointerException` is thrown. In the case of an empty element which means the collection etc. contains no elements, a `java.lang.IllegalArgumentException` is thrown (see above).

# References
* <http://commons.apache.org/proper/commons-lang/javadocs/api-release/org/apache/commons/lang3/Validate.html>
* <http://stackoverflow.com/questions/3881/illegalargumentexception-or-nullpointerexception-for-a-null-parameter>
