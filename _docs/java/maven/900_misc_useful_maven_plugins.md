---
title: Misc Useful Maven Plugins and Commands
tags: [maven]
category: maven
summary: "Misc Useful Maven Plugins and Commands"
---

# buildplan-maven-plugin

> A Maven 3.x plugin to inspect the lifecycle of your project.

* <http://buildplan.jcgay.fr/>
* <https://github.com/jcgay/buildplan-maven-plugin>

Example:
`mvn fr.jcgay.maven.plugins:buildplan-maven-plugin:list-phase` 


# Analyzing with SonarQube Scanner for Maven

* <https://docs.sonarqube.org/display/SCAN/Analyzing+with+SonarQube+Scanner+for+Maven>

Note: 
Configure sonar properties in `properties` area of maven `settings.xml` like: 
~~~xml
...
<!-- see https://docs.sonarqube.org/display/SCAN/Analyzing+with+SonarQube+Scanner+for+Maven -->
<sonar.host.url>http://192.168.56.101:9000</sonar.host.url>
...
~~~