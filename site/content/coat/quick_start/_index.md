---
title: "Quick Start"
weight: 1
---

Starting with Coat is easy.
Just write an interface with accessor methods for each field that should be
configurable. The return values of the accessor methods can be of any type.
Many types are already
[supported out of the box]({{< ref "/coat/user_guide/04_supported_types.md" >}}),
but it is possible to use
[custom types]({{< ref "/coat/user_guide/04_supported_types#registering-custom-types" >}}).

The source of the configuration data doesn’t matter. While it is
mainly intended to be used for the usual Java `.properties` files, it can
be used for any data that is composed of simple String-based
key-value-mappings.

Annotate the interface with the corresponding [Annotations]({{< ref
"/coat/user_guide/01_annotations.md" >}}) and let the Coat annotation processor
generate a concrete implementation of the interface.

That implementation can then be used to retrieve correctly typed config
values without any additional effort.

{{< button "01_prerequisites" "Prerequisites" >}}
