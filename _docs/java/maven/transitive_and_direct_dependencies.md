== danger ==
**WORK IN PROGRESS**
Simple collection of best practices etc.

# Transitive and direct dependencies

==info==
From <http://books.sonatype.com/mvnex-book/reference/optimizing-sect-dependency-plugin.html>:  
A good rule of thumb in Maven is to always declare explicit dependencies for classes referenced in your code.  

==success==

**(/) Exclude all transitive dependencies for a particular one**  
With maven 3.2.1 it is possible to exclude all transitive dependencies from a particular dependency with wildcard '*':

```xml
<exclusion>
    <groupId>*</groupId>
    <artifactId>*</artifactId>
</exclusion>
```
<https://jira.codehaus.org/browse/MNG-2315>

== ==


(un)related:
<http://www.smartics.de/archives/4692>
This is a short reminder for users of the smartics-jboss-modules-maven-plugin to add all direct dependencies to their project dependencies.  
If you use Java classes that are in the transitive closure, but not declared as dependencies directly in the projects POM, these classes will not be visible by default in the modules classpath provided by the JBoss AS 7. 

# References
* <http://maven.apache.org/>
* <http://books.sonatype.com/mvnref-book/reference/>
