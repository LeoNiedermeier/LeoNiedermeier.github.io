# Motivation
Sometimes it is desirable to write a simple file with maven. Examples are:
* version.txt file with the version and some other information
* ...
  

# Implementation

With the help of the maven-antrun-plugin and the ant echo task, it is quite easy to create a file:
1. the destination file is given with the file attribute of the echo task
2. the content of the file can be defined with the message attribute or the test of the element. But note that if the text of the element is used, whitespaces are written as given)

``` xml
<build>
  <plugins>
    <plugin>
      <groupId>org.apache.maven.plugins</groupId>
      <artifactId>maven-antrun-plugin</artifactId>
      <version>1.8</version>
      <executions>
        <execution>
          <goals>
            <goal>run</goal>
          </goals>
          <phase>prepare-package</phase>
          <configuration>
            <target if="maven.create.file">
              <echo file="${project.build.outputDirectory}/META-INF/version.txt" message="version: ${project.version}" />
            </target>
          </configuration>
        </execution>
      </executions>
    </plugin>
  </plugins>
</build>
```
## Conditionally create file
The target element supports among other the following attributes:
* if : The name of the property that must be set in order for this task. 
* unless: The name of the property that must NOT be set in order for this task.

If we want to create a file when the property maven.create.file is *set* to any value (not necessary 'true', it can also be 'false'), we write: 

``` xml
<target if="maven.create.file">
  ...
</target>
```

# References
* <http://maven.apache.org/plugins/maven-antrun-plugin/>
* <https://ant.apache.org/manual/Tasks/echo.html> 