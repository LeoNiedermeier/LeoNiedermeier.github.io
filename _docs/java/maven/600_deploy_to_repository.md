---
title: Deploy to Repository
tags: [maven]
category: maven
summary: "Deploy to Repository"
---

# Overview

> The deploy plugin is primarily used during the deploy phase, to add your artifact(s) to a remote repository 
> for sharing with other developers and projects. This is usually done in an integration or release environment.     
> (from <https://maven.apache.org/plugins/maven-deploy-plugin/>)

{: .info title="Minimum Version"}
Minimum version 2.8 of Apache Maven Deploy Plugin (<https://maven.apache.org/plugins/maven-deploy-plugin/>) required!

# Configuration

The repository locations are not configured in the `pom.xml` file. 
Instead we use the parameters / properties `altReleaseDeploymentRepository` and `altSnapshotDeploymentRepository`
(see <https://maven.apache.org/plugins/maven-deploy-plugin/deploy-mojo.html>). 
They can be defined as command line parameters or in the maven configuration file `settings.xml`:

{: .code title="Exmple of settings.xml"}
~~~xml
<settings>
  <servers>
    <server>
      <id>nexus-snapshots</id>
      <username>myUsername</username>
      <password>myPassword</password>
    </server>
    <server>
      <id>nexus-releases</id>
      <username>myUsername</username>
      <password>myPassword</password>
    </server>
  </servers>
  <profiles>
    <profile>
      <id>inject-application-properties</id>
      <properties>
        <altSnapshotDeploymentRepository>nexus-snapshots::http://192.168.56.101:8081/repository/maven-snapshots</altSnapshotDeploymentRepository>
        <altReleaseDeploymentRepository>nexus-releases::http://192.168.56.101:8081/repository/maven-releases</altReleaseDeploymentRepository>
      </properties>
    </profile>
  </profiles>
 
  <activeProfiles>
    <activeProfile>inject-application-properties</activeProfile>
  </activeProfiles>
</settings>
~~~


# References

* <https://maven.apache.org/plugins/maven-deploy-plugin/>
* <https://maven.apache.org/settings.html>