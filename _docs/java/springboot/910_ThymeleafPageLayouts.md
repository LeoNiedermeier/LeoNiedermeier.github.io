---
title: Thymeleaf Page Layouts
tags: [spring, spring_boot, thymeleaf]
category: spring
summary: "Thymeleaf Page Layouts"
---

# Overview

# Implementation

## Thymeleaf Layout Dialect

Uses Groovy

## With Bare Thymeleaf

As described in <http://www.thymeleaf.org/doc/articles/layouts.html> and in the 
Thymeleaf documentation <http://www.thymeleaf.org/doc/tutorials/3.0/usingthymeleaf.html#flexible-layouts-beyond-mere-fragment-insertion>
one can use fragments to build a common layout.

Defines the layout

~~~ html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org" th:fragment="template(title,content)">
<head>
  <title th:text="${title}">Layout</title>
  ...
</head>
<body>
  ...
    <div class="container">
        <div th:replace="${content}">A placeholder for real content.</div>
    </div>
...
</body>
</html>
~~~

Here we define a parametrized fragment `th:fragment="template(title,content)"` at root level. The fragment has two parameters

* title: a variable expression which is used for the title text
* content: fragment expression which is used to replace the content area: `th:replace="${content}"`

The layout template can be used as following

~~~ html
<html xmlns:th="http://www.thymeleaf.org" th:replace="fragments/layout :: template('My Title',~{::content})">
   ...
    <div class="row" th:fragment="content">
       ...
    </div>
</html>

~~~

# Other Implementations

Dynamic fragment inclusion based on a model variable. The view name is always the name of the main layout template, the fragment is selected by a model property.

Main layout:
~~~ html
...
<div th:replace="${freagmentToShow} :: content">
    Content
</div>
...
~~~

This approach is described in more detail by

* http://blog.codeleak.pl/2013/11/thymeleaf-template-layouts-in-spring.html
* https://github.com/jsumners/thymeleaf-layout-interceptor

# References

* <http://www.thymeleaf.org/doc/tutorials/3.0/usingthymeleaf.html#template-layout>
* <http://www.thymeleaf.org/doc/articles/layouts.html>
* <https://ultraq.github.io/thymeleaf-layout-dialect/>
