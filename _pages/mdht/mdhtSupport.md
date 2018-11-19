---
title: MDHT Support
layout: single
permalink: /docs/mdht/mdhtSupport/
toc: true
toc_icon: align-left  
---

The `ipf-modules-cda-mdht` module wraps the [MDHT] libraries from OpenHealthTools and provides
[Parser](../apidocs/org/openehealth/ipf/commons/core/modules/api/Parser.html),
[Renderer](../apidocs/org/openehealth/ipf/commons/core/modules/api/Renderer.html), and
[Validator](../apidocs/org/openehealth/ipf/commons/core/modules/api/Validator.html) implementations.

These implementations do not require Groovy to be used.

## Dependencies

Add the following depenedncy to the `pom.xml` file:

```xml
    <dependency>
       <groupId>org.openehealth.ipf.modules</groupId>
       <artifactId>ipf-modules-cda-mdht</artifactId>
       <version>${ipf-version}</version>
    </dependency>

```

## Camel integration

[MDHT-specific] Camel support is available in the [MDHT-specific] module.

## Examples

Here is an example how to parse and render a CCD document:

```java
   CDAR2Parser parser = new CDAR2Parser();
   CDAR2Parser renderer = new CDAR2Renderer();

   CDAR2Utils.initCCD();
   InputStream is = getClass().getResourceAsStream("/SampleCCDDocument.xml");
   ClinicalDocument clinicalDocument = parser.parse(is);
   String result = renderer.render(clinicalDocument, (Object[]) null);
```

Here is an example how to validate a CCD document:

```java
   CDAR2Validator validator = new CDAR2Validator();

   CDAR2Utils.initCCD();
   InputStream is = getClass().getResourceAsStream("/SampleCCDDocument.xml");
   ClinicalDocument clinicalDocument = parser.parse(is);
   validator.validate(clinicalDocument, null);
```

[MDHT]: https://www.projects.openhealthtools.org/sf/projects/mdht/
[MDHT-specific]: {{ site.baseurl }}{% link _pages/mdht/camelMdht.md %}