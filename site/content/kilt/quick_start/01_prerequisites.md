---
title: "Prerequisites"
weight: 1
---


## Basic prerequisites {#_basic_prerequisites}

-   Java 8 or higher

## To be integrated into an ant build script {#_to_be_integrated_into_an_ant_build_script}

When using Java 8 to 10:

-   Apache ant 1.8.1 or higher

When using Java 11+:

-   Apache ant 1.10.6 or higher [^1]

## To be integrated into a maven build {#_to_be_integrated_into_a_maven_build}

-   Apache maven 3.0.3 or higher


[^1]: Older version of ant can be used by exporting the environment
    variable `ANT_OPTS="-Djdk.util.jar.enableMultiRelease=force"`
