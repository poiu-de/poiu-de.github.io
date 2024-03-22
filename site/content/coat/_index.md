---
title: "Coat — Config of Annotated Types"
description: "Easy and typesafe config objects"
github_repo: "https://github.com/poiu-de/coat"
extra_menu_items:
  - example_project:
    label: "Example Project"
    url: "https://github.com/poiu-de/coat/tree/master/coat-example"
    icon: "devicon-github-original"
  - javadoc:
    label: "Javadoc"
    url: "https://javadoc.io/doc/de.poiu.coat/coat-runtime"
    icon: "devicon-java-plain"
weight: 1
---

# Generate typesafe config classes

Coat is an annotation processor to generate classes for reading
configuration values into typesafe objects.

## Short Usage

{{< block "grid-3" >}}
{{< column >}}
1. For the following `config.properties` file
```properties
appName     = My shiny app
listenPort  = 5040
description = Only a test project
 
 
 
 
 
 
 
```
{{< /column >}}

{{< column >}}

2. Define a corresponding interface
```java
import de.poiu.coat.annotation.Coat;

@Coat.Config
public interface MyConfig {
  public String appName();

  public int listenPort();

  public Optional<String> description();
}
```

{{< /column >}}

{{< column >}}
3. Then use the generated class
```java
final MyConfig config=
  MyConfigBuilder.from(
    new File("/path/to/config.properties"));

final String appName    = config.appName();
final int    listenPort = config.listenPort();
config.description().ifPresent(
  …
);
 

```
{{< /column >}}
{{< /block >}}


## Example project

See the
[Example project]({{< param coat_repo_url >}}/tree/master/coat-example)
for a fully executable mini application demonstrating the use of a Coat-generated
config class.

## License

Coat is licensed under the terms of the [Apache license 2.0](http://www.apache.org/licenses/LICENSE-2.0).

