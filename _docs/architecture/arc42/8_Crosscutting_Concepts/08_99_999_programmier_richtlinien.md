---
title: Allgemeine Programmierrichtlinien
tags: [arg42]
category: arc42
summary: "Allgemeine Programmierrichtlinien"
---
Wäre eine Zusammenfassung für die Umsetzung

# Formatierung
# Spring Dependency Injection


Siehe <https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#beans-dependencies>:
> #### Constructor-based or setter-based DI?
> Since you can mix constructor-based and setter-based DI, it is a good rule of thumb 
to use constructors for mandatory dependencies and setter methods or configuration methods for optional dependencies.     
...    
The Spring team generally advocates constructor injection, as it lets you implement 
application components as immutable objects and ensures that required dependencies are not null.    
...    
Setter injection should primarily only be used for optional dependencies that can be assigned reasonable default values within the class. 
...

<div class="danger" title="Direkt Field in Injection ist in jedem Falle zu vermeiden." markdown="1">

~~~java
public class MyClass {

    @Autowired
    private MyDependency myDependency;
}
~~~

</div>
# Tests
# Exceptions

# Logging
Siehe ...