---
title:  CI - Jenkinsfile für Build - Snapshot
tags: [arg42]
category: arc42
summary: "Jenkinsfile für SNAPSHOT Build"
---

# Übersicht

Der Snapshot Build wird als Multi-Branch-Pipeline angelegt. Dadurch kann für jeden vorhandenen Branch ein Snapshot Build erstellt 
werden.

# Maven Agent

Maven als docker agent (<https://jenkins.io/doc/book/pipeline/syntax/#agent>):

~~~
agent {
  docker {
    image 'maven:3-alpine'
    args '-v /root/.m2:/root/.m2 --network=host'
  }
}
~~~

## Docker Maven Images
Findet man unter <https://hub.docker.com/_/maven/>.

## Weiterführende Dokumentation 
* <https://jenkins.io/doc/book/pipeline/docker/>
* <https://jenkins.io/doc/book/pipeline/syntax/#agent/>

# Build Stage
~~~
stage('Maven: clean deploy') {
  steps {
    configFileProvider([configFile(fileId: 'jenkins-maven-settings', variable: 'MAVEN_SETTINGS')]) {
      sh 'mvn fr.jcgay.maven.plugins:buildplan-maven-plugin:list-phase   -Dbuildplan.tasks=clean,deploy'
      sh 'mvn -s $MAVEN_SETTINGS clean deploy'
    }
  }
  post {
    always {
      junit allowEmptyResults: true, testResults: 'target/surefire-reports/*.xml' 
    }
  }
}  
~~~


Hinweise: 
*  Der `agent` docker könnte auch nur im Build Stage definiert werden.  
*  Beim Anlegen der Konfigurationsdatei wurde die ID "jenkins-maven-settings" gesetzt.
* `post`: <https://jenkins.io/doc/book/pipeline/syntax/#post>  
* `junit`: Archive JUnit-formatted test results: (<http://192.168.56.101:8080/job/io.github.leoniedermeier.jenkins.demo/pipeline-syntax/html>)
* Das Maven Pipeline Plugin wird nicht benutzt, falls man es doch benutzen will, sollte man berücksichtigen:
  * Nur bei `mavenSettingsConfig` werden credentials ersetzt, bei `globalMavenSettingsConfig` nicht 
  * Unter `withMaven` besonderer Aufruf von Maven, z.B. mit `$MVN_CMD` (<https://github.com/jenkinsci/pipeline-maven-plugin/blob/master/jenkins-plugin/src/resources/faq.md#how-to-use-the-pipeline-maven-plugin-with-docker-since-version-303>)
  * Vgl. Dokumentation zu `withMaven` und was alles gemacht wird (<https://wiki.jenkins.io/display/JENKINS/Pipeline+Maven+Plugin>).


# TODO

* Lib wie <https://github.com/apache/maven-jenkins-lib/blob/master/vars/asfMavenTlpStdBuild.groovy>
  
# Referenzen
* <https://wiki.jenkins.io/display/JENKINS/Pipeline+Maven+Plugin>
* <https://github.com/jenkinsci/pipeline-maven-plugin/blob/master/jenkins-plugin/src/resources/faq.md#how-to-use-the-pipeline-maven-plugin-with-docker-since-version-303>