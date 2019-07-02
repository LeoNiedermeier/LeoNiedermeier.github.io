---
title: Test Tips
tags: [spring, spring_boot]
category: spring
summary: "Test Tips for Spring Boot"
---

## Setup with RestTemplateBuilder

Set up a `RestTemplateBuilder` in unit tests without psring application context.

~~~ java
MockServerRestTemplateCustomizer mockServerCustomizer = new MockServerRestTemplateCustomizer();
MyService myService = new MyServiceService(new RestTemplateBuilder(mockServerCustomizer));
MockRestServiceServer server = mockServerCustomizer.getServer();
~~~

See also javadoc of `MockServerRestTemplateCustomizer`


## Request / Response with MockRestServiceServer

Use

* `org.springframework.test.web.client.match.MockRestRequestMatchers`
* `org.springframework.test.web.client.response.MockRestResponseCreators`

~~~ java
 this.server.expect(MockRestRequestMatchers.requestTo("..."))
             .andRespond(MockRestResponseCreators.withSuccess(..., MediaType.APPLICATION_JSON));
~~~

## Disable Security

~~~ java
@WebMvcTest( secure = false)
~~~

or exclude autoconfiguration:

~~~ java
@WebMvcTestexcludeAutoConfiguration = { MockMvcSecurityAutoConfiguration.class })
~~~
