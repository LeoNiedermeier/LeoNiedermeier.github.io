---
title: Enums and Annotations
tags: [java, java_misc]
category: java_misc
summary: "Enum constants with annotations"
---

{: .code title="Example enum constant with annotation"}
~~~
enum MyEnum {
    @MyAnnotation("123")
    WITH_ANNOTATION,

    WITHOUT_ANNOTATION;
}
~~~

In order to retrieve the annotation of an enum constant, one should remember that an 
enum constant is a field of the concrete enum class. The field name is the name of the enum constant.

{: .code title="get annotation of enum constant"}
~~~
MyEnum enumConstant = ...
Class<MyAnnotation> annotation = ...

MyAnnotation result = enumConstant.getClass().getField(enumConstant.name()).getAnnotation(annotation);
    
~~~