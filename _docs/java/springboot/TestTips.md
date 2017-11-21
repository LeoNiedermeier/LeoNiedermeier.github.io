---
title: Test Tips
tags: [spring, spring_boot]
category: spring
summary: "Test Tips for Spring Boot"
---

# Request / Response wirh `MockRestServiceServer`

Use

* `org.springframework.test.web.client.match.MockRestRequestMatchers`
* `org.springframework.test.web.client.response.MockRestResponseCreators`

~~~java
 this.server.expect(MockRestRequestMatchers.requestTo("..."))
             .andRespond(MockRestResponseCreators.withSuccess(..., MediaType.APPLICATION_JSON));
~~~

# Disable Security

~~~java
@WebMvcTest( secure = false)
~~~

or exclude autoconfiguration:

~~~java
@WebMvcTestexcludeAutoConfiguration = { MockMvcSecurityAutoConfiguration.class })
~~~
