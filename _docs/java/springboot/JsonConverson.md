---
title: JSON conversion with Jackson
tags: [spring, spring_boot]
category: spring
summary: "Spring boot metrics"
---

# LocalDateTime

Deserialize a long number which represents milliseconds from the epoch to a `java.time.LocalDateTime`. 
Unfortunately the build in class `com.fasterxml.jackson.datatype.jsr310.deser.LocalDateTimeDeserializer` does not support this. 
The `com.fasterxml.jackson.datatype.jsr310.deser.InstantDeserializer` supports millisecond timestamps when configured `com.fasterxml.jackson.databind.DeserializationFeature.READ_DATE_TIMESTAMPS_AS_NANOSECONDS` as `false`.
 
One possible solution: use a converter which converts a `long` to a `LocalDateTime`.
 
~~~ java
public static class LongToLocalDateTimeConverter extends StdConverter<Long, LocalDateTime> {
    public LocalDateTime convert(final Long value) {
        return Instant.ofEpochMilli(value).atZone(ZoneId.systemDefault()).toLocalDateTime();
    }
}
~~~

And add a `@JsonDeserialize` to the field:

~~~ java
@JsonDeserialize(converter = LongToLocalDateTimeConverter.class)
private LocalDateTime localDateTime;
~~~
 
