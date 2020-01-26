---
title: Gemeral Notes about Spring
tags: [spring, spring_boot]
category: spring
summary: "Gemeral Notes about Spring"
---

# Constructor-based or setter-based DI

> #### Constructor-based or setter-based DI?
> Since you can mix constructor-based and setter-based DI, it is a good rule of thumb 
to use constructors for mandatory dependencies and setter methods or configuration methods for optional dependencies.     
...    
The Spring team generally advocates constructor injection, as it lets you implement 
application components as immutable objects and ensures that required dependencies are not null.    
...    
Setter injection should primarily only be used for optional dependencies that can be assigned reasonable default values within the class. 
...    
(From Spring Reference Documentation <https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#beans-dependencies>



# MVC Controllers

> The first rule of thumb is when building web apps is to keep Spring controllers as light as possible. 
We can think of them as converters between HTTP traffic and our system.    
(From "Learning Spring Boot 2.0 - Second Edition" by Greg L. Turnquist <https://www.packtpub.com/application-development/learning-spring-boot-20-second-edition>)

* No Business Logic in Spring Controllers
* ???  


* http://dolszewski.com/spring/how-to-bind-requestparam-to-object/

* https://mincong-h.github.io/2019/04/07/understanding-iso-8859-1-and-utf-8/