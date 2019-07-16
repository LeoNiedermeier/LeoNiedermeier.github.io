---
title: Builder
tags: [java, java_misc]
category: java_misc
summary: "Builder"
---

# Overview

# Extensible Builder
 When we define an extensible builder, one "problem" is that the methods of the base builder have to define a return type of 
 the sub-type. This can be archived by an self-referencial generic
 (see <http://www.angelikalanger.com/GenericsFAQ/FAQSections/ProgrammingIdioms.html#FAQ205> 
 and <http://www.angelikalanger.com/GenericsFAQ/FAQSections/ProgrammingIdioms.html#FAQ206>)

{: .code .x title="Example extensible builder"}
~~~
class BaseBuilder<T extends BaseBuilder<T>> {
        
    T baseElement() {
        return (T) this;
    }
}
    
class ExtendedBuilder extends BaseBuilder<ExtendedBuilder> {
    ExtendedBuilder extendendElement() {
        return this;
    }
}

void testBaseBuilder() {
    BaseBuilder<?> builder = new BaseBuilder<>();
    builder.baseElement().build();
}
    
void testExtendedBuilder() {
    ExtendedBuilder builder = new ExtendedBuilder();
    builder.baseElement().extendendElement().build();
    builder.extendendElement().baseElement().build();
}
~~~


# Builder with Mandatory and Optional Elements

The builder has some mandatory and some optional elements.

* Divide the builder in mandatory and optional parts
* Every mandatory builder method is defined in an individual interface (besides the first one)
  * The first mandatory builder method is defined as a static method in the `Builder` class
  * The last mandatory builder method returns the interface of the optional builder methods
  * A mandatory builder method returns the interface for the next mandatory builder method
* The interface of the optional builder methods includes the `build()` method

{: .code .x title="Builder with Mandatory and Optional Elements"}
~~~
interface Mandatory_2 {
    Mandatory_3 mandatory_2();
}
    
interface Mandatory_3 {
    OptionalElements mandatory_3();
}
    
interface OptionalElements {
    String build();
        
    OptionalElements optional_1();
        
    OptionalElements optional_2();
        
    OptionalElements optional_3();
}

class Builder implements Mandatory_2, Mandatory_3, OptionalElements {

    public static Mandatory_2 mandatory_1() {
        return new Builder();
    }

    public String build() {
        return "MY RESULT";
    }

    public Mandatory_3 mandatory_2() { ... ; return this;}

    public OptionalElements mandatory_3() { ... ; return this;}

    public OptionalElements optional_1() { ... ; return this;}

    public OptionalElements optional_2() { ... ; return this;}

    public OptionalElements optional_3() { ... ; return this;}
  

// invocations
void test() {
    Builder.mandatory_1().mandatory_2().mandatory_3().build();
    Builder.mandatory_1().mandatory_2().mandatory_3().optional_1().build();
    Builder.mandatory_1().mandatory_2().mandatory_3().optional_3().build();
    Builder.mandatory_1().mandatory_2().mandatory_3().optional_1().optional_2().build();
    Builder.mandatory_1().mandatory_2().mandatory_3().optional_2().optional_1().optional_3().build();
}
~~~
# References