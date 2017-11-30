---
title: Gemeral Notes about Spring
tags: [spring, spring_boot]
category: spring
summary: "Gemeral Notes about Spring"
---

# Constructor-based or setter-based DI

> Since you can mix constructor-based and setter-based DI, it is a good rule of thumb to use constructors for mandatory dependencies and setter methods or configuration methods for optional dependencies.    
...    
(From Spring Reference Documentation <https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#beans-dependencies>


# MVC Controllers

> The first rule of thumb is when building web apps is to keep Spring controllers as light as possible. 
We can think of them as converters between HTTP traffic and our system.    
(From "Learning Spring Boot 2.0 - Second Edition" by Greg L. Turnquist <https://www.packtpub.com/application-development/learning-spring-boot-20-second-edition>)
