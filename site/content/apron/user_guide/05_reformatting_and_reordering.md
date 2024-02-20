---
title: Reformatting and Reordering
weight: 5
---

Since version 2.0.0 Apron provides a
`de.poiu.apron.reformatting.Reformatter` class that allows reformatting and
reordering the content of .properties files.

The specific behaviour when reformatting and reordering can be specified
via a `de.poiu.apron.reformatting.ReformatOptions` object.

For convenience the `de.poiu.apron.PropertyFile` class provides some
methods to reformat or reorder the entries in that PropertyFile.

## Reformatting

When reformatting a format string can be given to specify how to format
leading whitespace, separators and line endings. The default format
string is `<key> = <value>\n` for

  - no leading whitespace

  - an equals sign surrounded by a single whitespace on each side as
    separator

  - a `\n` (line feed) character as new line character

By default the keys and values of the reformatted files are _not_
modified. That means any special formatting (like insignificant
whitespace, newlines and escape characters) remain after reformatting.

This can be changed via the `reformatKeyAndValue` option in which case
these will be modified as well.

This is an example for reformatting a
PropertyFile:

``` java
// Create the ReformatOptions to:
//   - read and write with UTF-8 (which is the default anyway),
//   - reformat via a custom format string and 
//   - also reformat the keys and values.
final ReformatOptions reformatOptions= ReformatOptions.create()
    .with(UTF_8)
    .withFormat("<key>: <value>\r\n")
    .withReformatKeyAndValue(true)
    ;

// Create a Reformatter with the specified ReformatOptions
final Reformatter reformatter= new Reformatter(reformatOptions);

// Reformat a single .properties file according to the specified ReformatOptions
reformatter.reformat(new File("myproperties.properties"));
```

## Reordering

Reordering the content of .properties files can be done either by
alphabetically sorting the keys of the key-value pairs or by referring
to a template file in which case the keys are ordered in the same order
as in the template file.

Apron allows specifying how to handle non-property lines (comments and
empty lines) when reordering. It is possible to move them along with the
key-value pair that _follows_ them or the key-value pair that _precedes_
them or be just left at the same position as they are.

This is an example for reordering a
PropertyFile:

``` java
// Create the ReformatOptions to use that does not reorder empty lines and comments
final ReformatOptions reorderOptions= ReformatOptions.create()
  .with(AttachCommentsTo.ORIG_LINE)
  ;

// Create a Reformatter with the specified ReformatOptions
final Reformatter reformatter= new Reformatter(reorderOptions);

// Reorder a single .properties file alphabetically 
// according to the specified ReformatOptions
reformatter.reorderByKey(new File("myproperties.properties"));

// Reorder a single .properties file according to the order in another .properties file
// This time we want to reorder comments and empty lines along with the 
// key-value pair that follows them. This is possible by specifying a ReformatOptions 
// object when calling the corresponding reorder method.
reformatter.reorderByTemplate(
  new File("template.properties"),
  new File("someOther.properties"),
  reorderOptions.with(AttachCommentsTo.NEXT_PROPERTY)
);
```

