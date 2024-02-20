---
title: "Basic Usage"
weight: 3
---

The main important class in Apron is `de.poiu.apron.PropertyFile`. It
provides methods to create a new instance by reading a .properties file
from File or InputStream as well as methods for populating an instance
programmatically.

The main difference to the usual `java.util.Properties` is that this
class does not implement the `java.util.Map` interface and provides
access to the content of the PropertyFile in two different ways:

  - as key-value pairs

  - as Entries

The key-value pairs are the actual interesting content of the
.properties files and are the same as when read via
`java.util.Properties`. However, since PropertyFile is able to retain
all comments, blank lines and even the formatting of the file it stores
all this information in objects of type `Entry`. There are two
implementations of `Entry`:

  - **BasicEntry**  
    A non-key-value pair like a comment or an empty line

  - **PropertyEntry**  
    An actual key-value pair

The Entry objects store their content in _escaped_ form. That means all
whitespaces, linebreaks, escape characters, etc. are contained in
exactly the same form as in the written .properties file.

The key-value pairs instead contain the content in _unescaped_ form (as
you would expect from `java.util.Properties`).

To minimize confusion the _escaped_ values are stored as CharSequence
whereas the _unescaped_ values are stored as String.

A PropertyFile instance also allows writing its content back to disk. It
provides 3 methods (each in two variants) for doing so:

  - **overwrite**  
    Writes the contents of the PropertyFile to a new file or overwrite
    an existing file.

  - **update**  
    Update an existing .properties file with the values in the written
    PropertyFile.

  - **save**  
    Use either the above mentioned overwrite method if the given file
    does not exist or the update method if the file already exists.

The most interesting method is the `update` method, since this
differentiates PropertyFile from `java.util.Properties`. It actually
only updates the values of the key-value pairs without touching any
other formatting. Blank lines, comments, whitespaces and even escaping
and special formatting of the keys are not altered at all. Also the
order of the key-value pairs remains the same.

The behaviour when writing a PropertyFile can be altered by providing it
an optional `ApronOptions` object.

This is an example for a typical usage of PropertyFile as a replacement
for `java.util.Properties`:

``` java
// Read the file "application.properties" into a PropertyFile
final PropertyFile propertyFile= PropertyFile.from(
  new File("application.properties"));

// Read the value of the key "someKey"
final String someValue= propertyFile.get("someKey");

// Set the value of "someKey" to a new value
propertyFile.set("someKey", "aNewValue");

// Write the PropertyFile back to file by only updating the modified values
propertyFile.update(new File("application.properties"));
```

This is an example for a more advanced usage of PropertyFile that allows
accessing comment lines and explicitly formatted (escaped) entries:

``` java
// Read all Entries (that means BasicEntries as well as PropertyEntries)
final List<Entry> entries= propertyFile.getAllEntries();

// Add a comment line to this PropertyFile
propertyFile.appendEntry(new BasicEntry("# A new key-value pair follows"));

// Add a new key-value pair to this PropertyFile
// Be aware that by using appendEntry() it could be possible to insert
// duplicate keys into this PropertyFile. The behaviour is then undefined.
// It is the responsibility of the user of PropertyFile to avoid this.
// PropertyEntries contain their content in _escaped_ form. Therefore the
// Backslashes and newline character are not really part of the key and value
propertyFile.appendEntry(new PropertyEntry("a new \\\nkey", "a new \\\nvalue"));

// key-value pairs are _unescaped_. Therefore the following method call
// will return the string "a new value"
final String myNewValue= propertyFile.get("a new key");

// Specify an ApronOptions object that writes with ISO-8859-1 encoding
// instead of the default UTF-8.
final ApronOptions apronOptions= ApronOptions.create()
  .with(java.nio.charset.StandardCharsets.ISO_8859_1);

// Write the PropertyFile back to file by only updating the modified values
propertyFile.update(new File("application.properties"), apronOptions);
```

See the [Javadoc API](https://javadoc.io/doc/de.poiu.apron/apron/) for
more details.

