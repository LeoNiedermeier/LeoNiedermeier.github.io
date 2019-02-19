---
title: Maven, Versions and Git
tags: [maven,git]
category: maven
summary: ""
---

# Overview

## The Need for Different Maven SNAPSHOT Versions

If we work with differnt git branches, it can be useful to build identifiable artifacts from different branches. For instance the different branches can be related to individual (test) environments.

# Assumptions

* Only CI Server deploys to artifact repository
* Artifacts from different branches sould be distinguishable by their version
* Should work in dev environemnt like eclipse

# Implementation

* For all `pom.xml` the version is `<version>local-SNAPSHOT</version>`
* The CI server can set individual versions

# Versioning with CI Friendly Versions

See <https://maven.apache.org/maven-ci-friendly.html>

`pom.xml`
~~~xml
<groupId>...</groupId>
<artifactId>...</artifactId>
<version>${revision}</version>
  ...
<properties>
  <revision>local-SNAPSHOT</revision>
</properties>
~~~

In a multi module setup, the child module's `pom.xml` references the parent's

~~~xml
...
<parent>
  <groupId>...</groupId>
  <artifactId>...</artifactId>
  <version>${revision}</version>
</parent>
...
~~~

{: .success title="Single Place"}
With the CI friendly versions there is only one place in a maven multi module setup, where a concrete version is defined. The parent's version is identified by the ${revision} property. 


### Eclipse
The `${revision}` is resolved to the configured value, in our case to "local-SNAPSHOT".

# CI SNAPSHOT Builds from Branches

In order to identify SNAPSHOT artifacts of the different branches, the maven version is the name of the branch + "-SNAPSHOT". Examples:

* master: master-SNAPSHOT
* feature-123: feature-123-SNAPSHOT

The version can be set as maven command line argument `-Drevision=my-version`. 

# Dependency Management for Development

## Multi Module Setup

### Development
In a multi module setup all modules have the same version. The version of another module from the setup is specified by `${project.version}` (see <https://maven.apache.org/maven-ci-friendly.html>).

### CI SNAPHSOT Feature Build
All modules of the multi module setup have the same version which refernces to the revsion value. 

### Merge
There are no version changes in the different `pom.xml` files.

## Multiple Independend Modules

### Development

* Modules: 
  * m_1 and m_2, can be in different git repositories
  * m_2 depends an m_1 
* Checked out with the same feature branch (same name)
* In the `pom.xml` od m_2 the version of the m_1 dependency is changed to ${project.version}

# CI SNAPHSOT Feature Build
CI must ensure, that the builds are called with the same revision value.

### Merge
In this case, each independend module is merged individually. After each merge the dependency versions are adjusted




If multiple modules should be changed together, they have the same feature branch (maybe in different git repositories). 

If the module 



# CI Release Builds
