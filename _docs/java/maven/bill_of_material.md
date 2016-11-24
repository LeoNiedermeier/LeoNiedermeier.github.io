---
title: Bill of Material
tags: [maven]
category: maven
summary: ""
---

# Motivation

In larger environments, like corporate environments, we have a lot of modules with for instance

* some common dependencies
* gathered in the same deployment unit like a web application

In order to keep the dependencies and particularly the version numbers consistent, we can use the so called "Bill of Materials" mechanism.

# Implementation

Steps:

* Define a bill of materials
* Include it in the pom
* Define dependencies 

## Define a bill of materials

You define the dependencies of the bom in the dependencyManagement. There you can also define exclusions (for instance exclude commons logging from spring dependencies)

``` xml
<?xml version="1.0" encoding="UTF-8"?>
<project>
  <modelVersion>4.0.0</modelVersion>
  <groupId>org.springframework</groupId>
  <artifactId>spring-framework-excluded-commons-logging-bom</artifactId>
  <version>4.1.0.RELEASE</version>
  <packaging>pom</packaging>
  <dependencyManagement>
    <dependencies>
      <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-core</artifactId>
        <version>4.1.0.RELEASE</version>
        <exclusions>
            <exclusion>
                <groupId>commons-logging</groupId>
                <artifactId>commons-logging</artifactId>
            </exclusion>
        </exclusions>
      </dependency>
      <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-expression</artifactId>
        <version>4.1.0.RELEASE</version>
      </dependency>
  </dependencyManagement>
</project>
```

{: .success title="Version numbers"}  
The version numbers in the dependency definition are the version numbers used in the importing poms.

## Include it in the pom

The bom is referenced in a dependencyManagement dependency with

* `<type>pom</type>`
* `<scope>import</scope>`


``` xml
...
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
  	</dependencies>
  </dependencyManagement>
```

## Define dependencies

The dependencies can be defined as usual, but without version:

``` xml
...
<dependencies>
	<dependency>
		<groupId>org.springframework</groupId>
		<artifactId>spring-core</artifactId>
	</dependency>
</dependencies>
...
```

{: .success title="No version numbers"}  
Do **NOT** use version numbers in the dependency definition.

# References

* <http://maven.apache.org/guides/introduction/introduction-to-dependency-mechanism.html#Importing_Dependencies>