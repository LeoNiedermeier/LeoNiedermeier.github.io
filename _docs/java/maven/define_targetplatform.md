---
title: Target Platfporm
tags: [maven]
category: maven
summary: "Define a target platform for dependencies."
---

# Motivation

In larger environments, like corporate environments, we have at least three dependency layers

* external dependencies
* the corporate framework (depends on external)
* the particular application (depends on external and corporate framework)

Most of the modules in the dependency tree have a different versioning scheme and a different release cycle. If we want to build a new version of our particular application, we are faced with dependencies on the same module with different version.

Our application is deployed in an known and well defined environment like as a web application in application server. In such a case we have

* dependencies / libraries provided by the environment
* dependencies / libraries collected in a shared lib folder
* dependencies / libraries bundled with the application

These constraints should be taken into account when defining the maven poms.

# Implementation
## The Example
Our application is a spring based web application. The application consists of two maven projects with submodules:

**Project structure**
```
example.base
    |---- example.base.core
    |---- example.base.web

example.ui
    |---- example.ui.core
    |---- example.ui.web

```
Therefore we have the following dependencies
* spring
* slf4j as  logging framework
* javax.servlet (provided by server)

## Define a target platform

{: .info title="What is a Target Platform?"}
'Target Platform' is a term in eclipse plug-in development:  
*The Target Platform refers to the plug-ins which your workspace will be built and run against. It describes the platform that you are developing for.*
<http://help.eclipse.org/luna/index.jsp?topic=%2Forg.eclipse.pde.doc.user%2Fconcepts%2Ftarget.htm>

With a [bom](./bill_of_material.html) (alias target platform) we can define a set of dependencies for our project. The target platform contains all dependencies for your project and the necessary artifacts for the deployment.

{: .info title"Define versions"}
 The bom is the central point for defining versions.


**Example**

pom.xml of target platform

``` xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
	<modelVersion>4.0.0</modelVersion>
	<groupId>target</groupId>
	<artifactId>target.platform</artifactId>
	<version>2.0.4</version>
	<packaging>pom</packaging>
	<properties>
		<ui.bom>1.9.2</ui.bom>
		<base.bom>1.0.0</base.bom>
	</properties>
	<dependencyManagement>
		<dependencies>
			<dependency>
				<groupId>org.springframework</groupId>
				<artifactId>spring-framework-excluded-commons-logging-bom</artifactId>
				<version>4.1.0.RELEASE</version>
				<type>pom</type>
				<scope>import</scope>
			</dependency>
			<dependency>
				<groupId>slf4j</groupId>
				<artifactId>slf4j-bom</artifactId>
				<version>1.77</version>
				<type>pom</type>
				<scope>import</scope>
			</dependency>
			<dependency>
				<groupId>javax.servlet</groupId>
				<artifactId>servlet-api</artifactId>
				<version>2.4</version>
				<scope>provided</scope>
			</dependency>


			<dependency>
				<groupId>example.bom</groupId>
				<artifactId>example.bom.ui.core</artifactId>
				<version>${ui.bom}</version>
			</dependency>

			<dependency>
				<groupId>example.bom</groupId>
				<artifactId>example.bom.ui.web</artifactId>
				<version>${ui.bom}</version>
			</dependency>

			<dependency>
				<groupId>example.bom</groupId>
				<artifactId>example.bom.base.core</artifactId>
				<version>${base.bom}</version>
			</dependency>
			<dependency>
				<groupId>example.bom</groupId>
				<artifactId>example.bom.base.web</artifactId>
				<version>${base.bom}</version>
			</dependency>
		</dependencies>
	</dependencyManagement>
</project>
	
``` 


pom.xml of module

``` xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<groupId>example.bom</groupId>
	<artifactId>example.bom.ui</artifactId>
	<version>2.0.5</version>
	<packaging>pom</packaging>
	<modules>
		<module>core</module>
		<module>web</module>
	</modules>
	<dependencyManagement>
		<dependencies>
			<dependency>
				<groupId>example.bom</groupId>
				<artifactId>example.bom.ui.core</artifactId>
				<version>${project.version}</version>
			</dependency>
			<dependency>
				<groupId>example.bom</groupId>
				<artifactId>example.bom.ui.web</artifactId>
				<version>${project.version}</version>
			</dependency>
			
			<dependency>
				<groupId>target</groupId>
				<artifactId>target.platform</artifactId>
				<version>2.0.4</version>
				<type>pom</type>
				<scope>import</scope>
			</dependency>
		</dependencies>
	</dependencyManagement>
</project>
```



{: .info}
In the module pom we define the dependency management of the submodules. This overrides the dependency management of the target platform.

## Handling version changes of dependencies
If the example.base and example.ui projects have different release cycles, it is likely that we have different versions of the target platform.  
**Example:**  
example.base depends on spring 3.2.1, example.ui depends on spring 3.2.4

### Change compatible versions
If we have to deal only with compatible versions, we can use the maven dependency resolution together with the exclusions of all transitive dependencies (see [Transitive and Direct Dependencies](./transitive_and_direct_dependencies.html))


### Change incompatible versions
In this case we have to check, whether we have to change elements (code, configuration file, etc.) of the module. If not, we have nothing to do, else we have to build a new version.

###  Different inherited versions
If we use 3rd party libraries with dependencies to other libraries, it can happen that an other common library is used by this libraries, but with different versions.
* compatible version: use the newest
* incompatible versions: we can use only one of the libraries with the common dependency. In some cases the libraries may work together, for instance because they do not depend on incompatible code. But this is a very risky assumption.

**Example**
* A (our application) depends on B and C (3rd party libraries) 
* B depends on X version 1.0.0
* C depends on X version 2.0.0
* X version 1.0.0 and X version 2.0.0 are incompatible

So: Maybe application A compiles, but due to the version clash it is likely that we get some runtime errors or some unwanted runtime behavior. The only secure solution is to remove on of the dependencies B or C.
