---
title: CI - Jenkinsfile
tags: [arg42]
category: arc42
summary: "Jenkinsfile Beispiele"
---

# Übersicht

* Maven basierte Java Projekte sollten gebaut werden.
* Kodanalyse mit Sonar
* Deployment der erzeugten Artefakte in ein Nexus Repository     

# Maven Konfiguration

## Voraussetzung


Installierte Plugins

* Config File Provider Plugin:  <https://wiki.jenkins.io/display/JENKINS/Config+File+Provider+Plugin>
  * Damit wird via settings.xml maven konfiguriert
    * Zugangsdaten für Nexus server
    * Diverse Konfigurations-Properties  
  * Anlegen von Konfigurationsfiles über "Manage Jenkins" -> "Manage Files"
  
( * Pipeline Maven Plugin: <https://wiki.jenkins.io/display/JENKINS/Pipeline+Maven+Plugin> )
  
## Maven settings.xml

{: .code title="settings.xml" .x}
~~~xml
<settings>
  <servers>
    <!-- Ersetzt durch Credential Konfiguration-->
    <server>
      <id>nexus-snapshots</id>
      <username>to be replaced</username>
      <password>to be replaced</password>
    </server>
    <server>
      <id>nexus-releases</id>
      <username>to be replaced</username>
      <password>to be replaced</password>
    </server>
  </servers>

  <!--  -->
  <mirrors>
    <mirror>
      <id>internal-repository</id>
      <name>Internal Maven Repository</name>
      <url>http://192.168.56.101:8081/repository/maven-public</url>
      <mirrorOf>*</mirrorOf>
    </mirror>
  </mirrors>
  <profiles>
    <profile>
      <id>inject-application-properties</id>
      <properties>
        <!-- URL für Ditributionmanagement -->
        <nexus-url>http://192.168.56.101:8081</nexus-url>
        <!-- see https://docs.sonarqube.org/display/SCAN/Analyzing+with+SonarQube+Scanner+for+Maven -->
        <sonar.host.url>http://192.168.56.101:9000</sonar.host.url>
      </properties>
    </profile>
  </profiles>
 
  <activeProfiles>
    <activeProfile>inject-application-properties</activeProfile>
  </activeProfiles>
</settings>
~~~ 

{: .info title="Hinweis"}
Eine ähnliche `settings.xml` Datei wird vom Entwickler benutzt. Diese wird üblicherweise in `<user_home>/.m2` abgelegt.

## Jenkinsfile


### Allgemein: 

Maven als docker agent (<https://jenkins.io/doc/book/pipeline/syntax/#agent>):

~~~
agent {
  docker {
    image 'maven:3-alpine'
    args '-v /root/.m2:/root/.m2 --network=host'
  }
}
~~~
#### Docker Maven Images
Findet man unter <https://hub.docker.com/_/maven/>.

#### Weiterführende Dokumentation 
* <https://jenkins.io/doc/book/pipeline/docker/>
* <https://jenkins.io/doc/book/pipeline/syntax/#agent/>

### Build Stages
~~~
pipeline {
  agent {
    docker {
      image 'maven:3-alpine'
      args '-v /root/.m2:/root/.m2 --network=host'
    }
  }
  stages {
    stage('Build') {
      steps {
         withMaven(mavenSettingsConfig : 'jenkins-maven-settings') {
          // https://github.com/jenkinsci/pipeline-maven-plugin/blob/master/jenkins-plugin/src/resources/faq.md#how-to-use-the-pipeline-maven-plugin-with-docker-since-version-303
          sh '$MVN_CMD help:effective-settings'
        }
      }
    }
  }
}
~~~


Hinweise: 
* Beim Anlegen der Konfigurationsdatei wurde die ID "jenkins-maven-settings" gesetzt.
* Nur bei `mavenSettingsConfig` werden credentials ersetzt, bei `globalMavenSettingsConfig` nicht 
* Unter `withMaven` besonderer Aufruf von Maven, z.B. mit `$MVN_CMD`

Statt `withMaven` kann man auch `configFileProvider` verwenden:
~~~
configFileProvider([configFile(fileId: 'jenkins-maven-settings', variable: 'MAVEN_SETTINGS')]) {
  sh 'mvn -s $MAVEN_SETTINGS help:effective-settings''
}
~~~

Vgl. Dokumentation zu `withMaven` und was alles gemacht wird (<https://wiki.jenkins.io/display/JENKINS/Pipeline+Maven+Plugin>).


# Referenzen
* <https://wiki.jenkins.io/display/JENKINS/Pipeline+Maven+Plugin>
* <https://github.com/jenkinsci/pipeline-maven-plugin/blob/master/jenkins-plugin/src/resources/faq.md#how-to-use-the-pipeline-maven-plugin-with-docker-since-version-303>