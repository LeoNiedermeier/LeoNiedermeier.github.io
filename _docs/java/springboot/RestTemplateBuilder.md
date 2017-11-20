---
title: The RestTemplateBuilder Class
tags: [spring, spring_boot]
category: spring
summary: "The RestTemplateBuilder Class"
---

# Overview

# Usage in Tests

## Setup of RestTemplateBuilder Programatically

Need to bind a `RestTemplate` to a `MockRestServiceServer`. 
The `bind` method sets a `MockClientHttpRequestFactory` as `ClientHttpRequestFactory` in the `RestTemplate`. 
This `MockClientHttpRequestFactory` has to be used by the `RestTemplate` created by the `RestTemplateBuilder`. 

~~~java
RestTemplate restTemplate = new RestTemplate();
MockRestServiceServer server = MockRestServiceServer.bindTo(restTemplate).build();
RestTemplateBuilder builder = new RestTemplateBuilder().requestFactory(restTemplate.getRequestFactory());
...
~~~

## Modul Tests


# References
