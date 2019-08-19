---
title: TypeUtils
tags:
  - java
  - apache_commons
category: apache_commons  
summary: Some example usages of the org.apache.commons.lang3.reflect.TypeUtils class.
---

# Overview

The `org.apache.commons.lang3.reflect.TypeUtils` class provides some interesting utility methods in particular with regard to 
generics.

# Examples

## Determine the Type Arguments of an Implemented Interface

This can be useful for framework like interfaces where the type arguments of the implementation / extension define the concrete 
use case.

~~~java
interface MyInterface extends Runnable, Supplier<String> {}

...
Map<TypeVariable<?>, Type> typeArguments = TypeUtils.getTypeArguments(MyInterface.class, Supplier.class);

// get the TypeVariable of Supplier (has exactly one):
TypeVariable<Class<Supplier>> tv = Supplier.class.getTypeParameters()[0];

// The typeArguments map holds the type assignements with respect to the class analyzed hierarchy.
Type type = typeArguments.get(tv);

System.out.println(type);
// output: class java.lang.String
~~~

With a two-liner we can get the actual type arguments:

~~~
public static List<Type> getTypes(final Type type, final Class<?> toClass) {
    Map<TypeVariable<?>, Type> typeArguments = TypeUtils.getTypeArguments(type, toClass);
    return Stream.of(toClass.getTypeParameters()).map(typeArguments::get).collect(Collectors.toList());
}
~~~

Example:
~~~java
interface MyInterface extends BiFunction<Integer, List<String>, Supplier<String>> {}

List<Type> types = getTypes(MyInterface.class, BiFunction.class);
// [class java.lang.Integer, java.util.List<java.lang.String>, java.util.function.Supplier<java.lang.String>]

// Note: java.util.List<java.lang.String> and  java.util.function.Supplier<java.lang.String> 
// are of type java.lang.reflect.ParameterizedType
~~~

# References

* <http://commons.apache.org/proper/commons-lang/javadocs/api-3.9/org/apache/commons/lang3/reflect/TypeUtils.html>