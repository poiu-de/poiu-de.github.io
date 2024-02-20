---
title: "Installation"
weight: 2
---

## Maven plugin {#_maven_plugin}

To use the maven plugin of Kilt include the following plugin section in
the pom of your project:

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
        <executions>
          <execution>
            <id>i18n-facade-generation</id>
            <goals>
              <goal>create-facade</goal>
            </goals>
          </execution>
        </executions>
      </plugin>
      …
    <plugins>
  <build>
```

See [the Kilt User Guide](/kilt/user_guide) for a detailed description of the
available goals and configuration parameters.

## Ant task {#_ant_task}

To use the ant task of Kilt download the kilt-ant package from the
[Download]({{< param kilt_repo_url >}}/releases) section and either use the
integrated `build.xml` file or use it as a sample to include it in your own ant
build script.

You will need the accompanied properties file and lib directory as well.

## As standalone tool {#_as_standalone_tool}

To use Kilt as a standalone tool download the kilt-cli package from the
[Download]({{< param kilt_repo_url >}}/releases) section and unpack it to
a directory of your choice.
