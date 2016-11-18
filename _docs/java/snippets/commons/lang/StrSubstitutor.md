---
title: StrSubstitutor
tags: [java, apache_commons]
category: apache_commons
summary: "Some example usages of the org.apache.commons.lang3.StrSubstitutor class."
---
# Overview

> Substitutes variables within a string by values.  
This class takes a piece of text and substitutes all the variables within it. The default definition of a variable is `${variableName}`. The prefix and suffix can be changed via constructors and set methods.

# Some usage scenarios
```java
// Define variables in a map
Map<String, Object> valueMap = new HashMap<>();
valueMap.put("fruit", "apple");
valueMap.put("color", "green");
// template string + variables = resolved string

String replaced = StrSubstitutor.replace("The ${fruit} is ${color} and ${shape:-round}.", valueMap);
// The apple is green and round.
```

* default values (per default separated by a `:-`)
* prefix and suffix can be customized (instead of `${` and `}`)
* recursive substitutions

# References
<http://commons.apache.org/proper/commons-lang/javadocs/api-release/org/apache/commons/lang3/text/StrSubstitutor.html>
