---
title: Maven and Git
tags: [maven,git]
category: maven
summary: ""
---

# Overview

Git can be used with many different workflows, see for instance <https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow>. Usually you work with a master branch and different feature branches.

# Git Branches and Maven Version Scheme

The maven version is the branch name in git plus "-SNAPSHOT". So the branch "my-feature-branch" has the maven version "my-feature-branch-SNAPSHOT". 

kramdown
: A Markdown-superset converter

Maruku
:     Another Markdown-superset converter

Advantages
: Simple mapping between git branch and maven version
: Can easily checked with maven constraints

Drawbacks

* No "traditional" maven version scheme


-> git maven plugin, add commit id to manifest.mf
-> check whether branch name in version

# Build Release

## The Maven Release Plugin

<http://maven.apache.org/maven-release/maven-release-plugin>

> Preparing a release goes through the following release phases:
>
* Check that there are no uncommitted changes in the sources
* Check that there are no SNAPSHOT dependencies
* Change the version in the POMs from x-SNAPSHOT to a new version (you will be prompted for the versions to use)
* Transform the SCM information in the POM to include the final destination of the tag
* Run the project tests against the modified POMs to confirm everything is in working order
* Commit the modified POMs
* Tag the code in the SCM with a version name (this will be prompted for)
* Bump the version in the POMs to a new value y-SNAPSHOT (these values will also be prompted for)
* Commit the modified POMs
>
> from <http://maven.apache.org/maven-release/maven-release-plugin/examples/prepare-release.html>

This has the following disadvantages

* Version changes. e.g. fom 1.0.0-SNAPSHOT to 1.0.0 to 1.0.1-SNAPSHOT in the branch.
* Here commit means also push to origin. In the case the release build fails, there are changes in origin without need.

## Build on Tags


Advantages

* No artificial commits in the branch.

Drawbacks

* Tag more difficult to find in history

## Steps

* Checkout from git
* set maven version
* Tag with "RELEASE"
* commit but do not push
* build with maven
* push to git repo


# Release with Jenkins

# References
* <https://axelfontaine.com/blog/final-nail.html>
* <https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow>
* <http://maven.apache.org/maven-release/maven-release-plugin/examples/perform-release.html>
