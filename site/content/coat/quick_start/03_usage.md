---
title: "Usage"
weight: 3
---

### Create config interface

Write an interface with accessor methods for each config entry your application
supports. The accessor methods can return the concrete types you want your
config entry to be. There is a number of types that are
[supported by default]({{< ref "/coat/user_guide/04_supported_types.md" >}}),
but custom types can be
[registered]({{< ref "/coat/user_guide/04_supported_types#registering-custom-types" >}})
to support additional types.

Config values that are _optional_, must be of type `java.util.Optional` or the
more specialized variants `OptionalInt`, `OptionalLong` or `OptionalDouble`.
All other config values are considered _mandatory_. Missing mandatory values
will fail validation on construction time and therefore raise an exception
(with details about the validation failures).

The interface _must_ be annotated with the `@Coat.Config` annotation for
the annotation processor to recognize it.

Each accessor _may_ be annotated with the `@Coat.Param` annotation
to tell the processor the corresponding key in the config file (if it
should be different than what Coat would infer otherwise) or a default
value in case the key is missing in the config file.

Both annotations have some possible attributes that can be set which are described in
[Annotations]({{< ref "/coat/user_guide/01_annotations.md" >}}).

For example:

```java
package com.example;

import de.poiu.coat.annotation.Coat;

@Coat.Config
public interface AppConfig {
  public String      appName();

  public InetAddress remoteIP();

  @Coat.Param(default = "5044")
  public int         remotePort();

  @Coat.Param(key = "long_description")
  public Optional<String> description();
}
```

### Generate the builder for the config class

When compiling the project the annotation processor will produce a builder to
build a concrete implementation of the interface in the same package and (by
default) the same name with `Builder` appended to it. Therefore the above
example interface would produce an `com.example.AppConfigBuilder` class.

### Use the generated builder

The generated builder can be instantiated with a number of different config
sources. Since version 1.0.0 Coat also allows instantiation from multiple
sources at the same time.

#### Config Sources

The main source of config entries for Coat are `java.util.Map<String, String>`s.
For common config sources some shortcut methods exist that provide
direct support, but in all other cases everything that can be represented as
a map of String keys to String values can be used as a config source.

java.util.Map<String, String>

: A map with key-value mappings can directly be given to Coat to instantiate
  a Config class with these entries.

java.io.File

: The traditional approach of specifying config entries in Java is via
  Property-Files. Coat explicitly supports this use case by allowing a `File`
  object as config source which in then read via Javas loading mechanism for
  Property-Files.

java.util.Properties

: Instead of specifying a file, a `Properties` object can be directly fed into
  Coat as config source. This allows, for example, the usage of Java System
  Properties as config source by reading them via `System#getProperties()`.

  While `java.util.Properties` can theoretically contain non-String keys or
  values, Coat does not allow these and will drop such entries (generating
  a warning message).

Environment variables

: Special support is provided for reading config entries from environment
  variables. This is a common approach for applications running in containers.

  Unfortunately the allowed character set for environment variable keys is much
  stricter than the character set in Coat config (and therefore Java Properties)
  files. For that reason a relaxed mapping is applied to match environment variables to Coat config keys.

  - All dots and hyphens are treated as underscores.
  - All uppercase characters in Coat config keys are preceded by an underscore (to convert camelCase to UPPER_CASE).
  - The comparison between the environment variables and the Coat config keys is done case insensitively.

  For example the environment variable `SERVER_MQTT_HOST` will match the config key `server.mqttHost`.

#### Instantiation via static factory methods

Static factory methods are provided for reading config values from a single config source:

- `from(java.util.Map)`
- `from(java.util.Properties)`
- `from(java.io.File)`
- `fromEnvVars()`

Example:

```java
final MyConfig config= MyConfigBuilder.from(System.getProperties());
```

#### Instantiation with multiple config sources

The generated builder allows for using multiple config sources by using
different `add` methods.

The order in which the config sources are read is defined by the order in which
they are added to the builder. Later config sources overwrite values with the
same keys of earlier config sources.

For example for reading the basic config from a config file, but allow
overriding some entries via environment variables:

```java
final MyConfig config= MyConfigBuilder.create()
  .add(new File("myConfig.properties"))
  .addEnvVars()
  .build();
```

#### Validation of config objets

Since version 2.0.0 config objects are always validated at construction time.
Therefore all the static factory methods as well as the `#build()` method of
the generated Builder throw a `ConfigValidationException` in case this validation
fails.
See [Validation]({{< ref "/coat/user_guide/03_validation.md" >}}) for a more
detailled description of the validation process.

```java
public class MyApp {
  public static void main(String[] args) {
    try {
      final AppConfigBuilder config= AppConfigBuilder.from(
        new File("/path/to/config.properties"));

      System.out.println("Starting " + config.appName());
      config.description().ifPresent(System.out::println);

      final Socket s= new Socket(config.remoteIP(), config.remotePort());

      …
    } catch (final ConfigValidationException ex) {
      System.err.println("Error in config file:\n"
                         + ex.getValidationResult().toString());
      System.exit(1);
    }
  }
}
```
