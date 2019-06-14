---
title: Docker, Jenkins, Sonarqube in Virtual Box
tags: [misc]
category: misc
summary: "Setup a VirtualBox for development with Jenkins and Sonarqube"
---

# General


# Setup

<https://funnelgarden.com/sonarqube-jenkins-docker/>



Follow the steps described in <https://github.com/galexandre/docker-jenkins-sonarqube>

# Connect from Host System

![Host Only Adapter](docker-jenkins-sonarqube/HostOnlyAdapter.png  "Host Only Adapter")

(<https://www.thomas-krenn.com/de/wiki/Netzwerkkonfiguration_in_VirtualBox#Bridged_networking>)


The Sonar Server can be accessed with `http://192.168.56.101:9000`

Maven  
~~~
mvn clean install sonar:sonar -Dsonar.projectKey=myProjectKey  -Dsonar.host.url=http://192.168.56.101:9000
~~~

# References

* <https://funnelgarden.com/sonarqube-jenkins-docker/>
* <https://github.com/galexandre/docker-jenkins-sonarqube>