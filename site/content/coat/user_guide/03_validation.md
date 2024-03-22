---
title: "Validation"
weight: 3
---

## Coat validation

On creating a config class from the generated builder the configuration will
always be validated in order to fail early in case the given configuration is
missing some values or existing values cannot be converted into the specified
type.

If the configuration is invalid, it will throw a
`ConfigValidationException`. This exception has a method
`getValidationResult()` that returns a value of type `ValidationResult`
that contains more information about the missing or wrong config values.

To issue an error message, the `toString()` method of the
`ValidationResult` can be used. For example

```java
try {
  MyConfig config= MyConfigBuilder.from(props);
} catch (final ConfigValidationException ex) {
  System.err.println("Error in config file:\n" 
                     + ex.getValidationResult().toString());
  System.exit(1);
}
```


## Java Bean Validation

While coat doesn’t provide explicit support for [Java Bean Validation](https://en.wikipedia.org/wiki/Bean_Validation) they can be used together.

Java Bean Validation allows for more specific constraints on the values in a config object, e.g. minimum and maximum values of an integer. Just add the appropriate annotations to the accessor methods and validate them with a `jakarta.validation.Validator`.

Be aware that Java Bean Validation _requires_ the usage of “get” prefixes on the accessor methods. Coat explicitly supports this use case by [stripping that prefix]({{< ref "/coat/user_guide/01_annotations.md#stripgetprefix" >}}) when inferring the config key by default.

See [the example project]({{< param coat_repo_url >}}/tree/master/coat-example) for a usage example of Java Bean Validation with Coat.
