---
title: CI - Jenkins Setup
tags: [arg42]
category: arc42
summary: "Jenkins Setup"
---

# Übersicht

# Setup
## Docker

~~~
docker run   -u root   --rm   -d   -p 8080:8080   -p 50000:50000   -v jenkins-data:/var/jenkins_home   -v /var/run/docker.sock:/var/run/docker.sock   jenkinsci/blueocean
~~~
## Zusätzliche Plugins

### Config File Provider Plugin
* <https://wiki.jenkins.io/display/JENKINS/Config+File+Provider+Plugin>
* Damit wird via settings.xml maven konfiguriert
  * Zugangsdaten für Nexus server
* Anlegen von Konfigurationsfiles über "Manage Jenkins" -> "Manage Files"


# Referenzen