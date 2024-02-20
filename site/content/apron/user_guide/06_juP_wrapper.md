---
title: java.util.Properties wrapper
weight: 6
---

Since version 2.1.0 Apron provides a
`de.poiu.apron.java.util.Properties` class as a wrapper to be used as a
drop-in replacement where a `java.util.Properties` object is required.

This wrapper derives from `java.util.Properties`, but uses an Apron
`PropertyFile` as the actual implementation.

## Example

To use it create it either via

``` java
de.poiu.apron.PropertyFile propertyFile= …
de.poiu.apron.java.util.Properties properties=
  new de.poiu.apron.java.util.Properties(propertyFile);
```

or via

``` java
de.poiu.apron.PropertyFile propertyFile= …
de.poiu.apron.java.util.Properties properties= propertyFile.asProperties();
```

All access via the `properties` object will then access to the
`propertyFile` object. Both objects can be used interchangebly to access
the actual contents.

## Differences to `java.util.Properties`

The wrapper tries to fulfil the `java.util.Properties` API as good as
possible. However there are a few differences:

  - `java.util.Properties` is derived from Hashtable and therefore
    non-String keys and values can be stored in it (although that is
    highly discouraged). As Aprons `PropertyFile` is not derived from
    Hashtable it doesn’t share this flaw. Therefore trying to use any
    other objects than Strings as keys or values will fail.

  - Aprons `PropertyFile` only supports key-value-based `.properties`
    files. As `java.util.Properties` also provides methods to read and
    write to XML files and those formats are not supported by Apron, the
    corresponding methods will always throw an
    UnsupportedOperationException.

  - `java.util.Properties` being derived from Hashtable is thread-safe.
    However Aprons `PropertyFile` is not thread-safe and therefore this
    wrapper is also not thread-safe.

