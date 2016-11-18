---
title: Maven Overview
tags: [maven]
category: maven
summary: "Overview maven"
---
== danger ==
**WORK IN PROGRESS**  
Simple collection of best practices etc.

# Naming
From <http://maven.apache.org/guides/mini/guide-naming-conventions.html>:

== info ==

**groupid** will identify your project uniquely across all projects, so we need to enforce a naming schema. It has to follow the package name rules, what means that has to be at least as a domain name you control, and you can create as many subgroups as you want  

**artifactId** is the name of the jar without version. If you created it then you can choose whatever name you want with lowercase letters and no strange symbols. If it's a third party jar you have to take the name of the jar as it's distributed.

== ==

If artifactId is the name of the jar, it has to be more or less unique. For  instance you can not deploy two jars with different groupId but the same artifactId in a web-inf/lib directory of a web application. Therefore one should avoid generic artifactIds like 'common' etc..

Consider a project with layer and slice architecture. The base packages can grouped like:

|        |Slice 1 | Slice 2 | Slice 3|
|--------|--------|--------|--------|
|Layer 1 | m.a.x  |  m.b.x |  m.c.x |
|Layer 2 | m.a.y  |  m.b.y |  m.c.y |
|Layer 3 | m.a.z  |  m.b.z |  m.c.z |

**Examples**  
a=common, b = shopping, c = catalog, x = ui, y = core, z = data  
* m.a.x = m.common.ui
* m.b.y = m.shopping.core
* m.c.z = m.catalog.data

-> jar names: m.x.jar or m.a.x.jar  
-> maven groupId:artifactId : m:a.x or m:m.a.x  

If m.a.z consists of more modules:
* m.a.z is the parent of the modules (has no source? -> no basepackage m.a.z?)
* m.a.z.s1, m.a.z.s2 etc. are modules



== success ==

In order to have unique jar names, the the jar name should be the jar's base package name (or the bundle symbolic name).  
* The maven artifactId is the base package name.  
* The maven groupId is part of the artifactId.  
* Split packages are not allowed and should not be neccesary.

== ==
<http://wiki.eclipse.org/MT4E_FAQ>



# References
* <http://maven.apache.org/>
* <http://books.sonatype.com/mvnref-book/reference/>
* <http://wiki.eclipse.org/MT4E_FAQ>
