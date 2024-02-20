---
title: "Kilt — Kilt I18n L10n T9n"
description: "Helper for handling Java I18n resource bundles"
github_repo: "https://github.com/poiu-de/kilt"
extra_menu_items:
  - example_project:
    label: "Example Project"
    url: "https://github.com/poiu-de/kilt/tree/master/kilt-example"
    icon: "devicon-github-original"
---

## Easier handling of Java I18n resource bundles

Kilt is a set of small tools to ease the handling of Java i18n resource bundles.

It can help by

 - Converting i18n resource bundles to and from XLS(X) sheets for easier translation by a small translation team.
 - Providing a facade to access the entries in a i18n resource bundle statically in a type safe way.
 - Reformat and reorder entries in resource bundles to maintain a constant style and order.

## Short Usage

### Create facade for accessing translation keys

{{< block "grid-3" >}}
{{< column >}}
1. For resource bundle `i18n/msg.properties`
```properties
ok      = OK
cancel  = Cancel
yes     = Yes
no      = No
 
 
 
```
{{< /column >}}

{{< column >}}

2. Generate the facade
```shell
 ./kilt.sh create-facade \
           -i msg.properties
 
 
 
 
 
```

{{< /column >}}

{{< column >}}
3. Then use the generated facade
```java
final I18n i18n= new I18n();

String s;
s = i18n.get(Msg.CANCEL);
s = i18n.get(Msg.OK);
Locale.setDefault(Locale.GERMAN);
s = i18n.get(Msg.CANCEL);
```
{{< /column >}}
{{< /block >}}

### Export XLS(X) sheet for translation

{{< block "grid-3" >}}
{{< column >}}
1. For all your resource bundles
```shell
i18n/msg.properties
i18n/msg_de.properties
i18n/msg_nl.properties
```
{{< /column >}}

{{< column >}}

2. Export them to a single XLS sheet
```shell
 ./kilt.sh export-xls \
           -i "msg*.properties"
 
```

{{< /column >}}

{{< column >}}
3. Then sent it to translators
```text
…
 
 
```
{{< /column >}}
{{< /block >}}

### Import XLS(X) sheet with translations

{{< block "grid-3" >}}
{{< column >}}
1. (Re)import XLS file with translations
```shell
 ./kilt.sh import-xls \
           -i "msg*.properties"
 
 
 
```
{{< /column >}}

{{< column >}}

2. This updates the resource bundles
```shell
i18n/msg.properties
i18n/msg_de.properties
i18n/msg_nl.properties
i18n/msg_fr.properties
 
```

{{< /column >}}

{{< column >}}
3. Then use the new translations
```java
final I18n i18n= new I18n();

String s:
Locale.setDefault(Locale.FRENCH)
s = i18n.get(Msg.CANCEL)
```
{{< /column >}}
{{< /block >}}


See the [User Guide](/kilt/user_guide) for a more thorough explanation of the possibilities.

## License

Kilt is licensed under the terms of the [Apache license 2.0](http://www.apache.org/licenses/LICENSE-2.0).
