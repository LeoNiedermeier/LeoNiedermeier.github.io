---
title: Snippets - Find class with annotation
tags: [spring, spring_boot]
category: spring
summary: "Find class with arbitrary annotation"
---

The
`org.springframework.context.annotation.ClassPathScanningCandidateComponentProvider`
can scan for classes with arbitrary annotations. 

{: .code  title="Find classes with annotation"}
~~~java
// A ClassPathScanningCandidateComponentProvider without default filters:
ClassPathScanningCandidateComponentProvider provider = new ClassPathScanningCandidateComponentProvider(false);

// Add a filter for the desired annotation (example: MyAnnotationTypeName)
provider.addIncludeFilter(new AnnotationTypeFilter(MyAnnotationTypeName.class));

// Find the classes under a given base package:
String basePackage = ...
provider.findCandidateComponents(basePackage) ....
~~~