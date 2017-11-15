---
title: Thymeleaf Notes
tags: [spring, spring_boot, thymeleaf]
category: spring
summary: "Thymeleaf Notes"
---

## Path Variables in URLs

~~~html
<a th:href="@{/stock/{symbol}/news/last/4(symbol=${item.symbol}) }" >...</a>
~~~

Conditional Rendering

Renders if "someNumber" is available. Note that "0" evaluates to false.
~~~html
th:if="${someNumber} != null"
~~~
