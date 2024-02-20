---
title: Logging
weight: 7
---

There are a few cases this library issues some logging statements (when
closing a writer didn’t succeed and if an invalid unicode sequence was
found that will be left as is). Those few logging statements don’t
justify a dependency on a logging framework. Therefore we just use
java.util.logging for that purpose.

When using Apron in an application that uses another logging framework
please use those logging frameworks ability to bridge java.util.logging
to their actual implementation.

For log4j2 this can be done by including the `log4j2-jul` and
`log4j2-api` jar (and some implemention, e.g. `log4j2-core`) and setting
the system property `java.util.logging.manager` to
`org.apache.logging.log4j.jul.LogManager`. See
<https://logging.apache.org/log4j/2.x/log4j-jul.html> for more
information.

For slf4j this can be done by including the `jul-to-slf4j` jar (and some
implementation, e.g. `logback`) and programmatically calling

``` java
SLF4JBridgeHandler.removeHandlersForRootLogger();
SLF4JBridgeHandler.install();
```

or setting the handler in the `logging.properties`:

``` xml
handlers = org.slf4j.bridge.SLF4JBridgeHandler
```

See <https://www.slf4j.org/legacy.html#jul-to-slf4j> for more
information.

