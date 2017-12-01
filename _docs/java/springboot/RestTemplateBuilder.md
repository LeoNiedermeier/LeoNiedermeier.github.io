---
title: The RestTemplateBuilder Class
tags: [spring, spring_boot]
category: spring
summary: "The RestTemplateBuilder Class"
---

# Overview

# Usage in Tests

## Setup of RestTemplateBuilder Programatically

This setup is useful for unit tests which should be independent of spring application context.
Like described in the javadoc of the class `org.springframework.boot.test.web.client.MockServerRestTemplateCustomizer`.
~~~ java
MockServerRestTemplateCustomizer mockServerCustomizer = new MockServerRestTemplateCustomizer();
MyService companyService = new MyService(new RestTemplateBuilder(mockServerCustomizer));
MockRestServiceServer server = mockServerCustomizer.getServer();
...
~~~

## Modul Tests

Test the module / applicaton without external dependencies. The setup corresponds to the setup of the spring boot application.

When using the `RestTemplateBuilder` there is a quite high possibility that there are more than one `RestTemplate`s created and used.
In order to build expectations against the `RestTemplate` invocations, one can use `MockServerRestTemplateCustomizer` like described in the javadoc. The disadvantage is that for each `RestTemplate` there is one `MockRestServiceServer` (and `RequestExpectationManager`) which make it difficult to formulate expactations. 

A easy way to circumvent this problem is to create a custom `RestTemplateCustomizer` which has similar functionality but uses only one `RequestExpectationManager` for all `RestTemplate`s.

~~~ java
public class MockRestServerRestTemplateCustomizer implements RestTemplateCustomizer {

    private final RequestExpectationManager expectationManager = new SimpleRequestExpectationManager();

    private final ClientHttpRequestFactory factory = (uri, httpMethod) -> {
        return new MockClientHttpRequest(httpMethod, uri) {
            @Override
            protected ClientHttpResponse executeInternal() throws IOException {
                return createResponse(this);
            }
        };
    };

    private ClientHttpResponse createResponse(final ClientHttpRequest clientHttpRequest) throws IOException {
        Assert.notNull(clientHttpRequest.getURI(), "'uri' must not be null");
        Assert.notNull(clientHttpRequest.getMethod(), "'httpMethod' must not be null");
        return this.expectationManager.validateRequest(clientHttpRequest);
    }

    @Override
    public void customize(final RestTemplate restTemplate) {
        restTemplate.setRequestFactory(this.factory);
    }
...
}
~~~


~~~ java
@RunWith(SpringRunner.class)
@SpringBootTest(classes = {..., MockRestServerRestTemplateCustomizer.class })
@AutoConfigureMockMvc
public class CMyTest {

    @Autowired
    private MockRestServerRestTemplateCustomizer mockServer;
    ...
    
     @Test
    public void test(){
      this.mockServer.expect(requestTo(...);
      ...
    }
~~~


# References
