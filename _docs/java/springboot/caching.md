---
title: Spring Boot Caching
tags: [spring, spring_boot]
category: spring
summary: "Spring boot Caching"
---

From [https://docs.spring.io/spring-boot/docs/2.0.x-SNAPSHOT/reference/html/boot-features-caching.html#boot-features-caching-provider-generic]

> "Generic caching is used if the context defines at least one `org.springframework.cache.Cache` bean. A `CacheManager` 
> wrapping all beans of that type is created."
	
  // oder KLasse:
		// org.springframework.boot.autoconfigure.cache.GenericCacheConfiguration
		// Also einfach Cache Beans erzeugen, der Rest geht von selbst.
		// Das ist nur notwendig, wenn verschiedene Konfigurationen für verschiedenen
		// Caches notwendig sind. Ansonsten könnte man das mit properties machen.

# References

* [https://docs.spring.io/spring-boot/docs/2.0.x-SNAPSHOT/reference/html/boot-features-caching.html#boot-features-caching-provider-generic]
