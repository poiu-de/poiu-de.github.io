---
title: "What is Apron"
weight: 1
---

Apron is a small library for reading and writing Java .properties files.
The main goal of this library is to be compatible with the
`java.util.Properties` class. Not API-wise (the API is quite different),
but being able to read every Java .properties file and getting exactly the
same key-value pairs as `java.util.Properties` does.

However Apron maintains the order of the entries in the properties files
and also the comments, blank lines and whitespace before keys and around
separators.

This allows writing .properties files back that do not differ from the
original ones.

Since version 2.0.0 Apron provides the ability to reformat and reorder
the content of .properties files according to different constraints.

Since version 2.1.0 Apron provides a wrapper to be
used as a (nearly) drop-in replacement for `java.util.Properties`.

Apron was mainly written to be used in the [Kilt
toolset]({{< param kilt_repo_url >}}),
but was intended from the start to be a general purpose library.
