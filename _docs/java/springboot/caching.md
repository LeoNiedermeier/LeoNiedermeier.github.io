---
title: Spring Boot Caching
tags: [spring, spring_boot]
category: spring
summary: "Spring boot Caching"
---

From <https://docs.spring.io/spring-boot/docs/2.0.x-SNAPSHOT/reference/html/boot-features-caching.html#boot-features-caching-provider-generic>

> Generic caching is used if the context defines at least one `org.springframework.cache.Cache` bean. A `CacheManager` 
> wrapping all beans of that type is created.

Or check the Javadoc of `org.springframework.boot.autoconfigure.cache.GenericCacheConfiguration`.

Therefore, if you have different configuration of caches, just create `Cache` beans. If the configuration is always the same, it can be done by the configuration properties of spring boot.

Example for Caffeine cache configuration bean:

~~~ java
import static java.util.concurrent.TimeUnit.MINUTES;

import org.springframework.cache.caffeine.CaffeineCache;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import com.github.benmanes.caffeine.cache.Caffeine;

@Configuration
public class MyCacheConfiguration {

	@Bean
	public CaffeineCache companyCache() {
		return new CaffeineCache("CACHE_NAME",
				Caffeine.newBuilder().expireAfterWrite(30, MINUTES).maximumSize(100).build());
	}
}
~~~

# References

* <https://docs.spring.io/spring-boot/docs/2.0.x-SNAPSHOT/reference/html/boot-features-caching.html#boot-features-caching-provider-generic>
