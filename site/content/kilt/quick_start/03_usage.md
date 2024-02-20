---
title: "Usage"
weight: 3
---

Kilt can be used in three different ways.

-   As a standalone application

-   As an ant task

-   As a maven plugin

## Standalone tool {#_standalone_tool}

To use Kilt as a standalone tool, download and unpack the kilt-cli
package from the [Download]({{< param kilt_repo_url >}}/releases)
page.

It contains a shell script for Linux, a batch file for Windows and a
kilt.properties file for the configuration.

To execute the tool run

    ./kilt.sh <command>

on Linux or

    kilt.bat <command>

on Windows.

You may override the configuration in the kilt.properties file by
specifying some properties as parameters to the command. For example to
create an XLS(X) sheet only for the english language run

    ./kilt.sh export-xls --include=**/*_en*.properties

See [Commands](/kilt/user_guide/01_commands) for a list of the available commands and
[Parameters](/kilt/user_guide/02_parameters) for a detailed description of the available
configuration parameters.

To show the usage help of the tool call it with the parameter `-h` or
`--help`:

    ./kilt.sh --help

To show the usage help of a specific command use the command `help`
followed by the required command:

    ./kilt.sh help export-xls

or specify the `-h` or `--help` flag after that command:

    ./kilt.sh export-xls --help

## Ant task {#_ant_task}

To use Kilt as an ant task, download and unpack the kilt-ant package
from the [Download]({{< param kilt_repo_url >}}/releases) page.

It contains a build.xml file and a kilt.properties file to be used
standalone (but still requires ant to be run) or as a sample to be
integrated into the build script of another application.

To execute a command run

    ant <command>

See [Commands](/kilt/user_guide/01_commands) for a list of the available commands and
[Parameters](/kilt/user_guide/02_parameters) for a detailed description of the available
configuration parameters.

## Maven plugin {#_maven_plugin}

You can use the maven plugin to import and export an XLS(X) sheet on the
fly, but since translations are usually an iterative process and will be
done more than once, it is much more common to configure the
kilt-maven-plugin for the project containing the Java i18n resource
bundle files.

However, usually it is not necessary to generate an XLS(X) sheet with
every build, therefore the corresponding maven goal is not bound to any
maven lifecycle phase by default.

To integrate the kilt-maven-plugin into your application include the
following plugin section in the pom of your project:

``` xml
  <build>
    <plugins>
      …
      <plugin>
        <groupId>de.poiu.kilt</groupId>
        <artifactId>kilt-maven-plugin</artifactId>
        <version>{{< param last_stable_kilt_version >}}</version>
        <configuration>
          …
        </configuration>
      </plugin>
      …
    <plugins>
  <build>
```

See [Parameters](/kilt/user_guide/02_parameters) for a detailed description of the
available configuration parameters.

To execute a goal run

    mvn kilt:<command>

You may override the configuration of the pom by specifying some
properties as parameters to the command. For example to create an XLS(X)
sheet only for the english language run

    mvn kilt:export-xls -Di18nIncludes="**/*_en*.properties"
