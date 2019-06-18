---
title: Maven Nexus Setup
tags: [arg42]
category: arc42
summary: "Maven Nexus Setup"
---

# Maven Setup

# User `settings.xml` Datei

Entsprechend den Empfehlungen aus <https://maven.apache.org/guides/mini/guide-mirror-settings.html> wird ein lokaler Nexus Repository 
Server als Proxy für maven central (<https://repo1.maven.org/maven2/>) angelegt. In der maven `settings.xml` 
Datei wird  folgende Konfiguration ergänzt:  

~~~xml
<mirrors>
  <mirror>
    <id>internal-repository</id>
    <name>Internal Maven Repository Manager</name>
    <url>http://192.168.56.101:8081/repository/maven-public</url>
    <mirrorOf>*</mirrorOf>
  </mirror>
</mirrors>
~~~

# Referenzen

* <https://maven.apache.org/guides/mini/guide-mirror-settings.html>