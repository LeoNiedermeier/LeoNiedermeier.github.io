---
title: Test Tips
tags: [spring, spring_boot]
category: spring
summary: "Test Tips for Spring Boot"
---

# Disable Security

~~~java
@WebMvcTest( secure = false)
~~~

or exclude autoconfiguration:

~~~java
@WebMvcTestexcludeAutoConfiguration = { MockMvcSecurityAutoConfiguration.class })
~~~
