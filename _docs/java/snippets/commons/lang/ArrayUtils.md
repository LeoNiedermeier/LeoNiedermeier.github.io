---
title: ArrayUtils
tags: [java, apache_commons]
category: apache_commons
summary: "Some example usages of the org.apache.commons.lang3.ArrayUtils class."
---
# Overview

# Some Methods

## ArrayUtils.nullToEmpty(...)

~~~java
String[] names = null;
~~~
When iterating over the array without a check, we get a `java.lang.NullPointerException`:

~~~java
for (String string : names) {
// ....
}
~~~

We can check the array with an if statement:

~~~java
if (names != null) {
  for (String string : names) {
    // ...
  }
}
~~~

Or we can use the `ArrayUtils.nullToEmpty(...)` method inside the for statement:

~~~java
for (String string : ArrayUtils.nullToEmpty(names)) {
}
~~~

{: .info title="Reduced complexity"}
With the use of `ArrayUtils.nullToEmpty(...)` instead of the `if` check, the complexity of the method is reduced (no extra execution branch) and you have to write less tests.

## Test for empty

* isEmpty
* isNotEmpty

## Add and remove elements
* add
* addAll
* remove
* removeAll
* removeElement

# References
<http://commons.apache.org/proper/commons-lang/apidocs/org/apache/commons/lang3/ArrayUtils.html>
