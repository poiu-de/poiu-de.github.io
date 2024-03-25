---
title: "Performance"
weight: 8
---

While performance never was a design goal of Coat it turned out that Coat actually has excellent performance. It is so fast compared to other config libraries that this section of the user manual was written to cover that detail.

On github a tool exists that already compares different config libraries. [This tool](https://github.com/joeljeremy7/java-config-library-benchmarks) also provides a a benchmark for Coat. It shows that Coat 1.0.0 already was faster than all other config libraries for Strings, but less so for ints. The reason is that Coat supports more sophisticated parsing rules for numeric values (with separating underscores) that have a performance penalty on parsing time.

But due to the immutable nature of Coat generated config classes this parsing does not need to be done after creating the config object, but can be integrated into the creation. Therefore Coat 2.0.0 addresses that issue and does all parsing on creation time of the object.

## Comparison

A full comparison using above mentioned tool, but providing [a more Coat-focused view](https://hupfdule.github.io/coat-performance-comparison) shows that _all_ accessors are now several orders of magnitude faster than all the other tested config libraries and even more than 4 times faster than reading simple Strings out of System Properties.

Due to that a Coat-generated config can be used directly even in the most performance-critical code paths without having to worry about storing config variables locally to
avoid bottlenecks when accessing a config variable very often.

### Runtime performance

The benchmark is extended to illustrate that different types don't impact the performance a significant way.

![Access time compared to other config libraries](../performance_overview.png)

The only benchmark standing out a bit (due to “only“ 70% of the performance of the other benchmarks) is the one involving redirection. It accesses an InetAddress object from am EmbeddedConfig object from the main config object:

![Detail view on Coat access time benchmark](../access_time.png)

```java
config.mqtt().hostName()
```

For comparison, both, accessing the EmbeddedConfig as a whole object and accessing the InetAddress directly from the EmbeddedConfig show the full 100% performance.
In real life a service needing access to the value of an embedded config will likely already have a reference to only that object instead of the main config object (that is what embedded configs were designed for) and don‘t need even that redirection.

### Creation performance

It is expected that the high performance at runtime (accessing the config values) comes with a much worse performance on creation time (reading and parsing the config values). Looking at the benchmark result the creation performance is still better than expected.

![Creation time compared to other config libraries](../creation_time.png)

A mini-config with a single int and a single string (like with the benchmarks for all the other config libraries) puts Coat on the third position, still faster than most alternatives. When adding more config entries, the time for constructing the object and especially parsing the config values increases. The example is a config with a list of 4 LocalDate objects and an embedded config object with an InetAddress.

Bear in mind that this creation time was only benchmarked out of curiosity and usually has no relevance in real life. Creation is done only once per config and profiling the [example application](https://github.com/poiu-de/coat/tree/master/coat-example) shows that the effort to prepare the Java Bean Validation takes more than 20 times longer than constructing the config object.
