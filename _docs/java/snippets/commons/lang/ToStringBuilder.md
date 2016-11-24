---
title: ToStringBuilder
tags:
  - java
  - apache_commons
category: apache_commons  
summary: Some example usages of the org.apache.commons.lang3.builder.ToStringBuilder class.
---

# Overview

{: .info title="Consistent string representation"}
>In Order to get a consistent string representation of the classes used in a project, we can
>
- use a toString generator, e.g eclipse 'Generate toString()...'
- use a builder mechanism in the `toString()` implementation.

>**From ToStringBuilder javadoc**
>
> - allowing field names
> - handling all types consistently
> - handling nulls consistently
> - outputting arrays and multi-dimensional arrays
> - enabling the detail level to be controlled for Objects and Collections
> - handling class hierarchies

# How to use

1. Create new ToStringBuilder with the current object: `new ToStringBuilder(this)`
2. Append items with `append(...)` methods
3. use `toString()`

```java
class Person {
        String name ="Joe";
        int age = 22;
        boolean smoker= true;

        @Override
        public String toString() {
            return new ToStringBuilder(this)
                    .append("name", name)
                    .append("age", age)
                    .append("smoker", smoker).toString();
        }
    }
```

outputs

```
Person@d2cc05a[name=Joe,age=22,smoker=true]
```

# The ToStringStyle class

The `ToStringStyle` class controls String formatting for `ToStringBuilder`. There are some predefined styles as constants in the `ToStringStyle` class:

- DEFAULT_STYLE
- MULTI_LINE_STYLE
- NO_FIELD_NAMES_STYLE
- SHORT_PREFIX_STYLE
- SIMPLE_STYLE

By subclassing the `ToStringStyle` class one can define custom styles.

# References

<http://commons.apache.org/proper/commons-lang/javadocs/api-release/org/apache/commons/lang3/builder/ToStringBuilder.html>
