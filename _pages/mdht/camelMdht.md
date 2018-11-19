---
title: Camel DSL Extensions for MDHT
layout: single
permalink: /docs/mdht/camelMdht/
toc: true
toc_icon: align-left  
---

Working with MDHT is explained in the [MDHT support] section.

However, [MDHT] `ClinicalDocument` objects are also transferred through Camel routes, and at times it is convenient
to have access to these APIs directly in Camel's routing DSL, e.g. to generate an acknowledge that shall be returned to
the client.


## Dependencies

The following dependency must be registered in `pom.xml`:

```xml

    <!-- IPF MDHT extensions and DSL -->
    <dependency>
        <groupId>org.openehealth.ipf.platform-camel</groupId>
        <artifactId>ipf-platform-camel-mdht</artifactId>
        <version>${ipf-version}</version>
    </dependency>

```


## IPF Extensions for MDHT support

### Data Format

MDHT documents can be parsed and rendered using a MDHT-specific Camel DataFormat, so you can use the
`unmarshal` and `marshal` functions provided by the Camel DSL. For an example see below.

### Validation

MDHT documents can be validated in routes with the `verify().mdht()` extension.

The Camel predicate can be used for filters or validators, however, by design it just returns `true` or `false`, and the
resulting [`PredicateValidationException`](https://camel.apache.org/maven/current/camel-core/apidocs/org/apache/camel/processor/validation/PredicateValidationException.html)
gives no details whatsoever about the details, i.e. *why* the MDHT validation has failed and the location of the failure in the document.

In contrast, the IPF validator throws a [`ValidationException`](../apidocs/org/openehealth/ipf/commons/core/modules/api/ValidationException.html)
containing all the details about the validation failure that was provided by the [MDHT support] validator classes.

```groovy

    from(...)
      .unmarshal().mdht()
      .verify().mdht()
      ...

```


[MDHT]: https://www.projects.openhealthtools.org/sf/projects/mdht/
[MDHT support]: {{ site.baseurl }}{% link _pages/mdht/mdhtSupport.md %}