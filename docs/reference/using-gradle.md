---
type: doc
layout: reference
title: "Using Gradle"
description: "This tutorials walks you through different scenarios when using Gradle for building applications that contain Kotlin code"
---

# Using Gradle

## Plugin and Versions

The *kotlin-gradle-plugin* compiles Kotlin sources and modules.

Define the version of Kotlin you want to use via *kotlin.version*. The possible values are:

* X.Y-SNAPSHOT: Correspond to snapshot version for X.Y releases, updated with every successful build on the CI server. These versions are not really stable and are
only recommended for testing new compiler features. Currently all builds are published as 0.1-SNAPSHOT. To use a snapshot, you need to [configure a snapshot repository
in the build.gradle file](#using-snapshot-versions).

* X.Y.Z: Correspond to release or milestone versions X.Y.Z, updated manually. These are stable builds. Release versions are published to Maven Central Repository. No extra configuration
is needed in your build.gradle file.

The correspondence between milestones and versions is displayed below:

<table>
<thead>
<tr>
  <th>Milestone</th>
  <th>Version</th>
</tr>
</thead>
<tbody>
{% for entry in site.data.releases.list %}
<tr>
  <td>{{ entry.milestone }}</td>
  <td>{{ entry.version }}</td>
</tr>
{% endfor %}
</tbody>
</table>

## Project Layout

The Kotlin plugin default project layout is illustrated by the table below

|  Directory | Meaning |
|  --- | --- |
| src/main/kotlin | Kotlin production sources |
| src/main/resources | Production resources |
| src/test/kotlin | Kotlin test sources |
| src/test/resources | Test resources |
| src/*sourceSetName*/kotlin | Kotlin sources for the given source set |
| src/*sourceSetName*/resources | Resources for the given source set |

Unlike Scala or Groovy plugins, java sources can not be place under *kotlin* directory. Please apply plugin *java* and use separate *java* directory.

## Configuring Dependencies

You need to add dependencies on kotlin-gradle-plugin and Kotlin standard library:

``` groovy

buildscript {
  ext.kotlin_version = "<version>"
  repositories {
    mavenCentral()
  }
  dependencies {
    classpath "org.jetbrains.kotlin:kotlin-gradle-plugin:$kotlin_version"
  }
}

apply plugin: "kotlin"

repositories {
  mavenCentral()
}

dependencies {
  compile "org.jetbrains.kotlin:kotlin-stdlib:$kotlin_version"
}
```

## Using Snapshot versions

If you want to use a snapshot version (nightly build), first add out snapshot repository and change the version to 0.1-SNAPSHOT:

``` groovy
buildscript {
  ext.kotlin_version = "0.1-SNAPSHOT"
  repositories {
    mavenCentral()
    maven {
      url 'http://oss.sonatype.org/content/repositories/snapshots'
    }
  }
  dependencies {
    classpath "org.jetbrains.kotlin:kotlin-gradle-plugin:$kotlin_version"
  }
}

apply plugin: "kotlin"

repositories {
  mavenCentral()
  maven {
    url 'http://oss.sonatype.org/content/repositories/snapshots'
  }
}

dependencies {
  compile "org.jetbrains.kotlin:kotlin-stdlib:$kotlin_version"
}
```


## Android Projects

Android's Gradle model is a little different from ordinary Gradle, so if you want to build an Android project written in Kotlin, you need
*kotlin-android* plugin instead of *kotlin*:

``` groovy

buildscript {
    ...
}
apply plugin: 'android'
apply plugin: 'kotlin-android'
```

### Android Studio

If you are using Android Studio, add the following under android:

``` groovy
android {
  ...

  sourceSets {
    main.java.srcDirs += 'src/main/kotlin'
  }
}
```

This lets Android Studio know that your kotlin directory is a source root, so when the project model is loaded into the IDE it will be properly recognized.

## Using External Annotations

External annotations for JDK and Android SDK will be configured automatically. If you want to add more annotations for some of your libraries, add the following line to your Gradle script:

``` groovy

kotlinOptions.annotations = file('<path to annotations>')
```

## Examples

For more examples, including Android, Mixed Java/Kotlin, [check out the Samples](https://github.com/JetBrains/kotlin-examples/tree/master/gradle) folder on GitHub
