---
title: Plugins - git commit id
tags: [maven]
category: maven
summary: "Maven Plugins: git commit id"
---

# Add git Informations with maven-git-commit-id-plugin

<https://github.com/git-commit-id/maven-git-commit-id-plugin>

Add git information to `META-INF/MANIFEST.MF`. The it is possible to trace back the content of the generated artifact (jar) to 
a git commit.

## Configuration

In the `<build> <plugins>` section of `pom.xml`:

~~~
<plugin>
    <groupId>pl.project13.maven</groupId>
    <artifactId>git-commit-id-plugin</artifactId>
    <version>2.2.6</version>
    <executions>
        <execution>
            <id>get-the-git-infos</id>
            <goals>
                <goal>revision</goal>
            </goals>
        <phase>initialize</phase>
        </execution>
    </executions>
    <configuration>
        <!-- everything else is default -->
        <generateGitPropertiesFile>false</generateGitPropertiesFile>
    </configuration>
</plugin>
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-jar-plugin</artifactId>
    <configuration>
        <archive>
            <manifestEntries>
                <git-branch>${git.branch}</git-branch>
                <git-commit-id>${git.commit.id.abbrev}</git-commit-id>
                <git-tags>${git.tags}</git-tags>
            </manifestEntries>
        </archive>
    </configuration>
</plugin>
~~~
## Output

Entries in `META-INF/MANIFESt.MF`
~~~
git-branch: master
git-commit-id: a1e95c1
git-tags: TEST-TAG-001

~~~