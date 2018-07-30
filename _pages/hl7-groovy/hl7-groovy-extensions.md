---
title: Functional Groovy extensions to HAPI
layout: single
permalink: /docs/hl7-groovy-extensions/
toc: true
toc_icon: align-left  
---


While the [HL7v2 DSL] has its focus on providing a domain-specific syntax to navigate in HL7 messages and changing fields
within messages, the functional extensions retrofit a couple of convenient functions on top of HAPI.
By means of Groovy metaprogramming, however, it looks like these extensions are part of the HAPI API, i.e. you can call
the methods on both the raw HAPI objects.

## [Custom model class factories][hl7v2cmcf]

In order to instantiate concrete implementations of Message, Group, Segment etc, the [HAPI] Parsers use a `ModelClassFactory`
member object that looks up classes for these model components. The default implementation provides access to model components
as specified in the HL7 specs.

In real world HL7 projects you frequently need to deal with non-standard HL7 "dialects" which are not covered by the specification
and causes the parser to generate "generic" model classes when used out-of-the-box.

Although [HAPI] already provides a [`CustomModelClassFactory`](https://hapifhir.github.io/hapi-hl7v2//base/apidocs/ca/uhn/hl7v2/parser/CustomModelClassFactory.html)
class to address this issue, IPF brings in some additional
flexibility, e.g. by compiling a [Groovy]-based `CustomModelClassFactory` at runtime.

Details are described [here][hl7v2cmcf].
{: .notice}

## [Creating new instances of messages, structures, and types][hl7v2creating]

You can create a new message from scratch by specifying event type, trigger event and version.
Just as creating a message, you can also create a segment, passing the enclosing [`Message`](https://hapifhir.github.io/hapi-hl7v2//base/apidocs/ca/uhn/hl7v2/model/Message.html)
object as argument, which determines the HL7 version to be used.
Finally, just as creating a message or segment, you can also create a composite or primitive field, passing the enclosing `Message`
object as argument.

Details are described [here][hl7v2creating].
{: .notice}


## [Mapping HL7 type values][hl7v2mapping]

The [Mapping Service] is part of the IPF Core features. After all, although often used in HL7 processing, code system mapping
is not a feature that is inherently exclusive for HL7.

What remains specific to IPF's HL7 v2 support, however, is that the mapping extensions can be applied directly on all [HAPI] types.

Details are described [here][hl7v2mapping].
{: .notice}

[HAPI]: https://hapifhir.github.io/hapi-hl7v2/
[Groovy]: https://www.groovy-lang.org
[Groovy extension module]: https://www.groovy-lang.org/metaprogramming.html#_extension_modules
[HL7v2 DSL]: ../hl7-groovy-dsl/
[hl7v2cmcf]: ../hl7-groovy-cmcf/
[hl7v2creating]: ../hl7-groovy-dslCreating/
[hl7v2mapping]: ../hl7-groovy-mapping/