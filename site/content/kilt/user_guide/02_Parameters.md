---
title: "Parameters"
weight: 2
---

Kilt supports the following parameters that can be set in the
configuration files for the standalone tool and the ant task and in the
plugin configuration of the maven plugin.

All parameters can additionally be given as command line parameters to
override the configuration for the standalone tool and the maven plugin.
The parameters of the ant task can not be overridden.

The following list describes the available parameters and to which
commands they apply.

| Parameter                                                | Description                                                                                               | `export-xls` | `import-xls` | `create-facade` | `reformat` | `reorder` |
|----------------------------------------------------------|-----------------------------------------------------------------------------------------------------------|--------------|--------------|-----------------|------------|-----------|
| [verbose](#_verbose)                                     | Whether to  generate more verbose output.                                                                 | ✔            | ✔            | ✔               | ✔          | ✔         |
| [propertiesRootDirectory](#_propertiesrootdirectory)     | The directory below which the i18n resource bundle files reside.                                          | ✔            | ✔            | ✔               | ✔          | ✔         |
| [i18nIncludes](#_i18nincludes)                           | The Java i18n resource bundles to include in the processing.                                              | ✔            | ✔            | ✔               | ✔          | ✔         |
| [i18nExcludes](#_i18nexcludes)                           | The Java i18n resource bundles to exclude from the processing.                                            | ✔            | ✔            | ✔               | ✔          | ✔         |
| [propertyFileEncoding](#_propertyfileencoding)           | The encoding of the Java i18n resource bundle files.                                                      | ✔            | ✔            | ✔               | ✔          | ✔         |
| [xlsFile](#_xlsfile)                                     | The XLS(X) file to export to / import from.                                                               | ✔            | ✔            |                 |            |           |
| [missingKeyAction](#_missingkeyaction)                   | What to do if the target file contains key-value pairs that do not exist in the input file.               |              | ✔            |                 |            |           |
| [facadeGenerationDirectory](#_facadegenerationdirectory) | The directory to write the generated file(s) to.                                                          |              |              | ✔               |            |           |
| [generatedPackage](#_generatedpackage)                   | The package name into which to generate the Java enum facade classes.                                     |              |              | ✔               |            |           |
| [copyFacadeAccessorClasses](#_copyfacadeaccessorclasses) | Whether to copy the facade accessor classes into the generated output.                                    |              |              | ✔               |            |           |
| [facadeAccessorClassName](#_facadeaccessorclassname)     | The class name to use when copying the facade accessor classes.                                           |              |              | ✔               |            |           |
| [format](#_format)                                       | The format to use for formatting the entries in the resource bundles.                                     |              |              |                 | ✔          |           |
| [reformatKeyAndValue](#_reformatkeyandvalue)             | Whether to reformat the keys and values themselves by removing insignificant whitespace and line breaks. |              |              |                 | ✔          |           |
| [byKey](#_bykey)                                         | Reorder the entries alphabetically by the name of their keys.                                             |              |              |                 |            | ✔         |
| [byTemplate](#_bytemplate)                               | Reorder the entries in the same order as the key-value pairs in this template file.                       |              |              |                 |            | ✔         |
| [attachCommentsTo](#_attachcommentsto)                   | How to handle comment lines and empty lines on reordering.                                                |              |              |                 |            | ✔         |

## `verbose` {#_verbose}

Print more verbose output.

Specifying this options lets Kilt print more informational messages
about what it is doing.

When using the maven plugin this also requires the option `-X` to let
maven actually print the additional output.

## `propertiesRootDirectory` {#_propertiesrootdirectory}

The location of the source i18n resource bundle files.

All resource bundles that are handled by Kilt must reside in this
directory (or any subdirectory of arbitrary depth).

In a maven application this will usually be `src/main/resources`.

## `i18nIncludes` {#_i18nincludes}

The Java i18n resource bundles to include in the processing.

File globbing is supported with the following semantics:

-   `?` matches a single character
-   `*` matches zero or more characters
-   `**` matches zero or more directories

For example if you have the following resource bundles:

-   `messages_de.properties`
-   `messages_en.properties`
-   `buttons_de.properties`
-   `buttons_en.properties`
-   `internal/exceptions_de.properties`
-   `internal/exceptions_en.properties`
-   `internal/messages.properties`
-   `internal/messages_en.properties`

these are the results for the following patterns\>

| Pattern                     | Resulting files                                              |
|-----------------------------|--------------------------------------------------------------|
| `**/*.properties`           | All properties files                                         |
| `messages*.properties`      | messages_de.properties <br/> messages_en.properties          |
| `**/messages_en.properties` | messages_en.properties <br/> internal/messages_en.properties |

## `i18nExcludes` {#_i18nexcludes}

The files to exclude from the list of resources bundles given in
[i18nIncludes](#_i18nincludes).

File globbing is supported with the same semantics as for the
`i18nIncludes`.

## `propertyFileEncoding` {#_propertyfileencoding}

The encoding of the Java i18n resource bundle files.

Prior to Java 9 the default encoding in Java was ISO-8859-1, since Java
9 it is UTF-8.

## `xlsFile` {#_xlsfile}

The XLS(X) file to export to / import from.

On export, if the file doesn’t exist already it will be created. If it
already exists it will be updated (retaining formatting and unrelated
content). It is advisable to let Kilt generate the first version of the
file before making manual changes, since Kilt expects a certain
structure of the file.

## `missingKeyAction` {#_missingkeyaction}

How to handle key-value-pairs that exist in the .properties file, but
not in the XLS(S) file to import.

The following values are valid:

| Value   | Description                               |
|---------|-------------------------------------------|
| NOTHING | Leave exising key-value-pairs as they are |
| DELETE  | Delete the missing key-value-pairs        |
| COMMENT | Comment out the missing key-value-pairs   |

## `facadeGenerationDirectory` {#_facadegenerationdirectory}

The directory to write the generated Java enum facade classes to.

The default value when using the maven plugin is
`${project.build.directory}/generated-sources/kilt` otherwise it is
`generated-sources`.

## `generatedPackage` {#_generatedpackage}

The package name into which to generate the Java enum facade classes.

## `copyFacadeAccessorClasses` {#_copyfacadeaccessorclasses}

Whether to copy the facade accessor class and the base interface
I18nBundleKey to the generation target dir.

This is only useful if it is necessary to avoid a runtime dependency on
kilt-runtime, which provides these classes.

## `facadeAccessorClassName` {#_facadeaccessorclassname}

The name of the facade accessor class when copying the facade accessor
classes.

This is only meaningful in combination with
[copyFacadeAccessorClasses](#_copyfacadeaccessorclasses).

## `format` {#_format}

The format to use when reformatting entries of resource bundles.

The given format string must conform to the following specification:

-   It may contain some leading whitespace before the key.
-   It must contain the string `<key>` to indicate the position of the
    properties key (case doesn’t matter)
-   It must contain a separator char (either a colon or an equals sign)
    which may be surrounded by some whitespace characters.
-   It must contain the string `<value>` to indicate the position of the
    properties value (case doesn’t matter)
-   It must contain the line ending char(s) (either `\n` or `\r` or
    `\r\n`)

The allowed whitespace characters are

-   the space character
-   the tab character
-   the linefeed character.

Therefore a typical format string is

    <key> = <value>\n

for

-   no leading whitespace
-   an equals sign as separator surrounded by a single whitespace
    character on each side
-   `\n` as the line ending char.

But it may as well be

    \t \f<key>\t: <value>\r\n

for a rather strange format with

-   a tab, a whitespace and a linefeed char as leading whitespace

-   a colon as separator char preceded by a tab and followed a single
    space character

-   \\r\\n as the line ending chars

If the format string is omitted the default value of `<key> = <value>\n`
will be used.

## `reformatKeyAndValue` {#_reformatkeyandvalue}

Whether to reformat the keys and values of reformatted entries by
removing insignificant whitespace and linebreaks.

## `byKey` {#_bykey}

Reorder the entries of resource bundles alphabetically by the name of
their keys.

This option may not be given at the same time as
[byTemplate](#_bytemplate).

## `byTemplate` {#_bytemplate}

Reorder the entries of resource bundles in the same order as the
key-value pairs in this template file.

This option may not be given at the same time as [byKey](#_bykey).

## `attachCommentsTo` {#_attachcommentsto}

How to handle comment lines and empty lines when reordering the entries
of resource bundles.

Possible values are:

| Value           | Desription                                                                  |
| --------------- | --------------------------------------------------------------------------- |
| NEXT_PROPERTY   | Comments and empty lines are attached to the key-value pair *after* them.   |
| PREV_PROPERTY   | Comments and empty lines are attached to the key-value pair *before* them.  |
| ORIG_LINE       | Comments and empty lines remain at their current position.                  |

