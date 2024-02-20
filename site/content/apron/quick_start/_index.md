---
title: Quick Start
weight: 1
---

## Prerequisites

Apron has no runtime dependencies on other libraries.

Apron can be used with Java 8 or higher.


## Installation

To use Apron in a maven based project use the following maven coordinates:

```xml
    <dependency>
      <groupId>de.poiu.apron</groupId>
      <artifactId>apron</artifactId>
      <version>{{< param last_stable_apron_version >}}</version>
    </dependency>
```

Otherwise download the jar-file of Apron from the
[Github Release page]({{< param apron_repo_url >}}/releases)
and put it into the classpath of your application.

## Usage

Create a `de.poiu.apron.PropertyFile` object from the content of a
.properties file. 

### Reading a .properties file

Apron is fully compatible with the format supported by
`java.util.Properties`, but allows accessing the key value pairs in a
more developer-friendly way.

``` java
// Read the file "application.properties" into a PropertyFile
final PropertyFile propertyFile= PropertyFile.from(
  new File("application.properties"));

// Read the value of the key "someKey"
final String someValue= propertyFile.get("someKey");
```

### Writing changes to a .properties file back

Even more important than reading is writing changes back into a .properties file.
Other than `java.util.Properies` Apron does retain

 - the order of all entries

 - all empty lines

 - all comment lines

 - and even the formatting of unchanged entries

```java
// Set the value of "someKey" to a new value
propertyFile.set("someKey", "aNewValue");

// Write the PropertyFile back to file by only updating the modified values
propertyFile.update(new File("application.properties"));
```
