---
title: "Apron — Advanced Properties"
description: "Read and write Java .properties files in a more sane manner"
github_repo: "https://github.com/poiu-de/apron"
extra_menu_items:
  - javadoc:
    label: "Javadoc"
    url: "https://javadoc.io/doc/de.poiu.apron/apron"
    icon: "devicon-java-plain"
---

# A better API for accessing Java .properties files

Apron is a library for providing a developer-friendly way for reading and
writing Java .properties files. In particular it supports writing changes to
a .properties file in the least intrusive way possible.


## Short Usage

{{< block "grid-3" >}}
{{< column >}}
1. Provide a .properties file
```properties
## Apron example ##

# The appName can be changed in-app
appName     = My shiny app

# The default port is 5040.
# Change it if necessary
#listenPort  = 5040

# This description is only visible in
# the "About" dialog.
description = Only a \
              test project
 
 
 
```
{{< /column >}}

{{< column >}}

2. Read it and modify some entries
```java
import de.poiu.apron.PropertyFile;


final File file= new File("application.properties");
final PropertyFile propertyFile= PropertyFile.from(file);

// Read the value of the key "appName"
final String appName= propertyFile.get("appName");

// Change the value of "appName" to a new value
propertyFile.set("appName", "My brilliant app");

// Write the PropertyFile back to file
// by only updating the modified values
propertyFile.update(file);
}
```

{{< /column >}}

{{< column >}}
3. Read the result
```properties
## Apron example ##

# The appName can be changed in-app
appName     = My brilliant app

# The default port is 5040.
# Change it if necessary
#listenPort  = 5040

# This description is only visible in
# the "About" dialog.
description = Only a \
              test project
 
 
 
```
{{< /column >}}
{{< /block >}}


## License

Apron is licensed under the terms of the [Apache license 2.0](http://www.apache.org/licenses/LICENSE-2.0).

