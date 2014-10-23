---
layout: post
title:  "Gradle Versioner plugin"
date:   2014-10-22 23:00:00
categories: gradle java jar
comments: yes
---

When working with Gradle you often end up with a long list of dependencies. Now the big question is, how can you easily determine if your dependencies are up to date ?
<a href="https://github.com/ben-manes/gradle-versions-plugin/" target="_blank">Gradle Versions Plugins</a> is here to the rescue, it adds a single task that can check out your list of dependencies and give you back report of which dependencies are using the latest version, which exceed the latest version and which have newer versions.

Example ouput: 

<pre>
------------------------------------------------------------
: Project Dependency Updates (report to plain text file)
------------------------------------------------------------

The following dependencies are using the latest integration version:
 - backport-util-concurrent:backport-util-concurrent:3.1
 - backport-util-concurrent:backport-util-concurrent-java12:3.1

The following dependencies exceed the version found at the integration revision level:
 - com.google.guava:guava [99.0-SNAPSHOT <- 16.0-rc1]
 - com.google.guava:guava-tests [99.0-SNAPSHOT <- 16.0-rc1]

The following dependencies have later integration versions:
 - com.google.inject:guice [2.0 -> 3.0]
 - com.google.inject.extensions:guice-multibindings [2.0 -> 3.0]
</pre>

The plugin can also be configured to output json or xml.