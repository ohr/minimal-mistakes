---
title: Documentation
layout: single
classes: wide
permalink: /docs/
header:
  overlay_color: "#000"
  overlay_filter: "0.1"
  overlay_image: /assets/images/documentation2.jpg
---

The following table summarizes the IPF features related to the eHealth domain:

| Feature                                              | Description                                                  |
| ---------------------------------------------------- | ------------------------------------------------------------ |
| [Support for eHealth integration profiles][]         | A set of components for creating actor interfaces as specified in IHE and Continua integration profiles. IPF currently supports creation of actor interfaces for the IHE profiles XDS, PIX, PDQ, PIXv3, PDQv3, PIXm, PDQm, MHD, QED, QEDm, XCPD, XCA, XCA-I, XCF, XPID, PCD, as well as for Continua profiles HRN and WAN. |
| HL7 Message processing with [Groovy][] or [Kotlin][] | Basis for HL7 message processing is the HL7v2 DSL.           |
| [HL7 Message translation][]                          | Translation utilities for translating between HL7v3 and HL7v2 messages for corresponding IHE transactions |
| [FHIR Support][]                                     | FHIR® – Fast Healthcare Interoperability Resources (hl7.org/fhir) – is a next generation standards framework created by HL7 leveraging the latest web standards and applying a tight focus on implementability. |
| [DICOM Audit Support][]                              | Support for constructing, serializing and sending DICOM audit messages |
| [CDA Support][]                                      | Wrapping a number of CDA-related libraries                   |


Other IPF features provide part of the underlying foundation or supporting functionality:

| Feature                          | Description                                                  |
| -------------------------------- | ------------------------------------------------------------ |
| [Core Features][]                | Domain-neutral message processors and DSL extensions usable for general-purpose message processing. |
| [Code System Mapping][]          | A simplistic mechanism for mapping code values between code systems |
| [Dynamic Feature Registration][] | Aids in building up modular integration solutions where each module contributes routes, services etc. to the overall application |


IPF comes with a number of [Spring Boot Starters][] that support running eHealth applications
in the Spring Boot runtime environment.


[Support for eHealth integration profiles]: ihe/
[Groovy]: hl7-groovy/
[Kotlin]: hl7-kotlin/
[HL7 Message translation]: ipf-commons-ihe-hl7v3/index.html
[DICOM Audit Support]: audit/
[FHIR support]: ipf-platform-camel-ihe-fhir-core/index.html
[CDA Support]: ipf-modules-cda/index.html
[CDA processing Camel routes]: ipf-platform-camel-cda/index.html
[Core Features]: ipf-platform-camel-core/index.html
[Code System Mapping]: ipf-commons-map/index.html
[Dynamic Feature Registration]: dynamic.html
[Spring Boot Starters]: boot/