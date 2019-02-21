---
title: Maven Plugins - Enforcers
tags: [maven]
category: maven
summary: " Useful Maven Enforcer Plugin Elements"
---

# Overview

<https://maven.apache.org/enforcer/maven-enforcer-plugin/index.html>

# Configuration
~~~xml
<build>
    <plugins>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-enforcer-plugin</artifactId>
        <version>3.0.0-M2</version>
        <executions>
          <execution>
            <id>enforce-versions</id>
            <goals>
              <goal>enforce</goal>
            </goals>
            <configuration>
              <rules>
                 <!-- Add rules here --->
              </rules>
            </configuration>
          </execution>
        </executions>
      </plugin>
    </plugins>
  </build>
~~~

# Enforce Managed Dependencies

<https://github.com/jdcasey/enforce-managed-deps-rule>

>  It simply checks that all dependencies in your POM have versions that were specified in a `<dependencyManagement>` section.

# The 'Illegal' Transitive Dependency Check Rule

<https://github.com/Scout24/illegal-transitive-dependency-check>

> The rule checks if all classes in a certain artifact references only classes that are provided by explicitly declared dependencies. 

HINWEIS: https://maven.apache.org/enforcer/enforcer-rules/banTransitiveDependencies.html ist was anderes


**NOTE**
Due to <https://github.com/Scout24/illegal-transitive-dependency-check/issues/30> add exclusion and dependency to pom.xml: 

see <https://github.com/Apicurio/apicurio-studio/blob/master/pom.xml>
~~~xml
<plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-enforcer-plugin</artifactId>
                <dependencies>
                    <dependency>
                        <groupId>de.is24.maven.enforcer.rules</groupId>
                        <artifactId>illegal-transitive-dependency-check</artifactId>
                        <version>1.7.4</version>
                        <exclusions>
                            <exclusion>
                                <groupId>org.ow2.asm</groupId>
                                <artifactId>asm-analysis</artifactId>
                            </exclusion>
                        </exclusions>
                    </dependency>
                    <dependency>
                        <groupId>org.ow2.asm</groupId>
                        <artifactId>asm-analysis</artifactId>
                        <version>6.1.1</version>
                    </dependency>
                </dependencies>
~~~


# Check the Encoding

<http://www.mojohaus.org/extra-enforcer-rules/requireEncoding.html>

# Ban Duplicate Classes

Duplicate classes on the classpath can be a major problem. Typically this happens, when the same classes are packed in different 
artifacts which are included through transient dependencies.

<http://www.mojohaus.org/extra-enforcer-rules/banDuplicateClasses.html>

# Especially for Release Builds

## Require Release Dependencies

<https://maven.apache.org/enforcer/enforcer-rules/requireReleaseDeps.html>

## Require Release Version

<https://maven.apache.org/enforcer/enforcer-rules/requireReleaseVersion.html>
