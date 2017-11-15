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

## th:block

`th:block`: Thymeleaf will execute these attributes and then simply make the block, but not its contents, disappear.
See <http://www.thymeleaf.org/doc/tutorials/3.0/usingthymeleaf.html#synthetic-thblock-tag>
