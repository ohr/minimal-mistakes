---
title: IPF components for eHealth integration profiles
layout: single
permalink: /docs/ihe/
classes: wide
---


IPF provides support for several eHealth profiles, primarily from the [IHE][] ITI domain.
The basic idea is to offer a [Camel component](https://camel.apache.org/components.html) for each profile transaction.
These components ensure that the technical requirements of the profile are met by applications built on top of the
IPF eHealth Integration Profiles support.

### Context

Core concepts of IHE profiles are:

| Concept          | IHE Definition (Technical Framework, Volume 1) | Description  | Example |
|------------------|------------------------------------------------|--------------|--------|
| Actor            | Actors are information systems or components of information systems that produce, manage, or act on categories of information required by operational activities in the enterprise. | An application role in a distributed system | Patient Identity Cross-Reference Manager aka PIX Manager. |
| Transaction      | Transactions are interactions between actors that communicate the required information through standards-based messages. 	                                                         | A message exchange between actors           | Patient Identity Deed between the Patient Identity Source and the PIX Manager |
| Profile          | Each integration profile is a representation of a real-world capability that is supported by a set of actors that interact through transactions.                                    | A set of actors and transactions            | PIX profile |

IPF is a development framework with special support for the implementation of IHE concepts (i.e. profiles).
This is illustrated with an abstract IHE profile the figure below.

The profile defines three actors and two transactions. Transaction 1 is between Actor 1 and Actor 2 whereas Transaction 2 
is between Actor 1 and Actor 3.

![Abstract IHE Profile](/../../assets/images/ihe.png)

With IPF, these IHE concepts are mapped to [Camel components](https://camel.apache.org/component.html), partly by reusing
or extended existing Camel components or by providing [custom components](https://camel.apache.org/writing-components.html).

| Concept          | Mapping to the IPF/Camel programming model |
|------------------|----------------------------------------------------------------------|
| Actor            | Producer or Consumer of a [Camel endpoint](https://camel.apache.org/endpoint.html) |
| Transaction      | Camel component, e.g. `pix-iti8` |
| Profile          | Group of Camel components, e.g. `pix`, `xds` |


IHE Profiles are grouped by their underlying technical foundation, particularly by their *message format* and
*transport protocol* into IPF modules that can be included as dependencies in the Maven project descriptor:

| Module                                  | Provided IHE transactions |
|-----------------------------------------|----------------------------------------- |
| ipf-platform-camel-ihe-mllp             | [ITI-8], [ITI-9], [ITI-10], [ITI-21], [ITI-22], [ITI-30], [ITI-31], [ITI-64] |
| ipf-platform-camel-ihe-xds              | [ITI-18], [ITI-38], [ITI-39], [ITI-41], [ITI-42], [ITI-43], [ITI-51], [ITI-57], [ITI-61], [ITI-62], [ITI-63], [ITI-86], [RAD-69], [RAD-75], [RMU ITI-X1] |
| ipf-platform-camel-ihe-hl7v3            | [ITI-44], [ITI-45], [ITI-46], [ITI-47], [ITI-55], [ITI-56], [PCC-1] |
| ipf-platform-camel-ihe-hpd              | [ITI-58], [ITI-59], [CH-PIDD] |
| ipf-platform-camel-ihe-fhir-stu3-mhd    | [ITI-65], [ITI-66], [ITI-67], [ITI-68] |
| ipf-platform-camel-ihe-fhir-stu3-pixpdq | [ITI-78], [ITI-83] |
| ipf-platform-camel-ihe-fhir-stu3-audit  | [ITI-81] |
| ipf-platform-camel-ihe-fhir-stu3-qedm   | [PCC-44] |
| ipf-platform-camel-ihe-hl7v2ws          | [PCD-01] |

## Example


An implementation of a *Patient Identity Cross-Reference Manager* actor for the *Patient Identity Feed* (PIX ITI-8) transaction
shall be created. PIX ITI-8 is a HL7v2-based transaction using MLLP as transport protocol.
Receiving actors are implemented as a [Camel consumer](https://camel.apache.org/maven/current/camel-core/apidocs/org/apache/camel/Consumer.html).

Thus, the following dependency must be registered in `pom.xml`:

```xml
    <dependency>
        <groupId>org.openehealth.ipf.platform-camel</groupId>
        <artifactId>ipf-platform-camel-ihe-mllp</artifactId>
        <version>${ipf-version}</version>
    </dependency>
```

The basic pattern for consumers is to specify the component name in the URI parameter of a `from`-clause at the beginning of a Camel route:

```java
    // IHEConsumer.java
    from("pix-iti8://0.0.0.0:8777?parameters....")
      .process(...)
      // process the incoming HL7v2 request and create a response
```

While stepping through the Camel route, a proper response (or an Exception) must be generated that is sent back to the caller.

On the other side of the transaction, sending actors (for ITI-8 the *Patient Identity Source*) are implemented as a
[Camel producer](https://camel.apache.org/maven/current/camel-core/apidocs/org/apache/camel/Producer.html).
The basic pattern for producers is to specify the component name in the URI parameter of a to-clause at the end of a Camel route:

```java
    // IHEProducer.java
    from(...)
      .process(...)
      // create a ITI-8 request (i.e. HL7v2 message)
      .to("pix-iti8://mpiserver.com:8888?parameters....")
      // optionally process the response
```


## Transaction Parameters

The parameters usually depend on the IHE transaction; some parameters are valid for groups of transactions,
a few are even valid for all transactions (e.g. turning ATNA auditing on or off). Details are given in the respective pages
describing each component in detail

The most IPF eHealth components are named according to the profile and the transaction they implements
(transaction IDs and profiles relate to IHE, when not stated otherwise).
A special case is the MLLP dispatcher component which allows to accept requests for multiple MLLP-based transactions through a single TCP port.


## Supported Transactions

The table below references all supported eHealth transactions. Click on the link in the first column for details about
required dependencies, usage and parameters.

| Transaction             | Profile          | Description           | IPF Component          | Transport          | Message Format  |
|:------------------------|:-----------------|:----------------------|:-----------------------|:-------------------|:----------------|
{% for hash in site.data.ihe -%}
  {% assign tx = hash[1] -%}
| [{{ tx.transaction }}]({{ tx.link }})  | {{ tx.profile }} | {{ tx.description }}  | `{{ tx.component }}`   | {{ tx.transport }} | {{ tx.format }} |
{% endfor %}


| Transaction  | Profile       | Description                          | IPF Component           | Transport     | Message Format |
|:------------ |:------------- |:-------------------------------------|:----------------------- |:------------- |:--------------|
| [ITI-8]      | PIX, XDS      | Patient Identity Feed                | `pix-iti8`, `xds-iti8`  | MLLP(S)       | HL7 v2.3.1 |
| [ITI-9]      | PIX           | PIX Query                            | `pix-iti9`              | MLLP(S)       | HL7 v2.5 |
| [ITI-10]     | PIX           | PIX Update Notfication               | `pix-iti10`             | MLLP(S)       | HL7 v2.5 |
| [ITI-18]     | XDS           | Registry Stored Query                | `xds-iti18`             | SOAP/HTTP(S)  | ebXML |
| ITI-19       | ATNA          | Authenticate Node                    | n/a                     | n/a           | n/a |
| ITI-20       | ATNA          | Record Audit Event                   | n/a                     | Syslog (+TLS) | DICOM/IHE Audit Message |
| [ITI-21]     | PDQ           | Patient Demographics Query           | `pdq-iti21`             | MLLP(S)       | HL7 v2.5 |
| [ITI-22]     | PDQ           | Patient Demographics and Visit Query | `pdq-iti22`             | MLLP(S)       | HL7 v2.5 |
| [ITI-30]     | PAM           | Patient Identity Management          | `pam-iti30`             | MLLP(S)       | HL7 v2.5 |
| [ITI-31]     | PAM           | Patient Encounter Management         | `pam-iti31`             | MLLP(S)       | HL7 v2.5 |
| [ITI-38]     | XCA           | Cross-Gateway Query                  | `xca-iti38`             | SOAP/HTTP(S)  | ebXML |
| [ITI-39]     | XCA           | Cross-Gateway Retrieve               | `xca-iti39`             | SOAP/HTTP(S)  | ebXML |
| [ITI-41], Continua HRN | XDS, XDM, XDR, Continua | Provide & Register Document Set | `xds-iti41`      | SOAP/HTTP(S)  | ebXML |
| [ITI-42]     | XDS           | Register Document Set                | `xds-iti42`             | SOAP/HTTP(S)  | ebXML |
| [ITI-43]     | XDS           | Retrieve Document Set                | `xds-iti43`             | SOAP/HTTP(S)  | ebXML |
| [ITI-44]     | PIXv3, XDS    | Patient Identity Feed v3             | `pixv3-iti44`,`xds-iti44` | SOAP/HTTP(S)| HL7v3 |
| [ITI-45]     | PIXv3         | PIX Query v3                         | `pixv3-iti45`           | SOAP/HTTP(S)  | HL7v3 |
| [ITI-46]     | PIXv3         | PIX Update Notification v3           | `pixv3-iti46`           | SOAP/HTTP(S)  | HL7v3 |
| [ITI-47]     | PDQv3         | Patient Demographics Query (PDQ) v3  | `pdqv3-iti47`           | SOAP/HTTP(S)  | HL7v3 |
| [ITI-51]     | XDS           | Multi-Patient Stored Query           | `xds-iti51`             | SOAP/HTTP(S)  | ebXML |
| [ITI-55]     | XCPD          | Cross-Gateway Patient Discovery      | `xcpd-iti55`            | SOAP/HTTP(S)  | HL7v3 |
| [ITI-56]     | XCPD          | Cross-Gateway Patient Location Query | `xcpd-iti56`            | SOAP/HTTP(S)  | HL7v3 |
| [ITI-57]     | XDS           | Update Document Set                  | `xds-iti57`             | SOAP/HTTP(S)  | ebXML |
| [ITI-58]     | HPD           | Provider Information Query           | `hpd-iti58`             | SOAP/HTTP(S)  | DSMLv2 |
| [ITI-59]     | HPD           | Provider Information Feed            | `hpd-iti59`             | SOAP/HTTP(S)  | DSMLv2 |
| [ITI-61]     | XDS           | Register On-Demand Document Entry    | `xds-iti61`             | SOAP/HTTP(S)  | ebXML |
| [ITI-62]     | RMD           | Remove Metadata                      | `rmd-iti62`             | SOAP/HTTP(S)  | ebXML |
| [ITI-63]     | XCF           | Cross-Gateway Fetch                  | `xcf-iti63`             | SOAP/HTTP(S)  | ebXML |
| [ITI-64]     | XPID          | Notify XAD-PID Link Change           | `xpid-iti64`            | MLLP(S)       | HL7 v2.5 |
| [ITI-65]     | MHD           | Provide Document Bundle              | `mhd-iti65`             | REST/HTTP(S)  | FHIR |
| [ITI-66]     | MHD           | Find Document Manifests              | `mhd-iti66`             | REST/HTTP(S)  | FHIR |
| [ITI-67]     | MHD           | Find Document References             | `mhd-iti67`             | REST/HTTP(S)  | FHIR |
| [ITI-68]     | MHD           | Retrieve Document                    | `mhd-iti68`             | HTTP(S)       | HTTP |
| [ITI-78]     | PDQm          | Patient Demographics Query for Mobile | `pdqm-iti78`           | REST/HTTP(S)  | FHIR |
| [ITI-81]     | ATNA          | Retrieve ATNA Audit Event            | `atna-iti81`            | REST/HTTP(S)  | FHIR |
| [ITI-83]     | PIXm          | Patient Identifier Cross-reference for Mobile | `pixm-iti83`   | REST/HTTP(S)  | FHIR |
| [ITI-86]     | RMD           | Remove Documents                     | `rmd-iti86`             | SOAP/HTTP(S)  | ebXML |
| [RMU ITI-X1] | RMU           | Restricted Update Document Set       | `rmu-itiX1`             | SOAP/HTTP(S)  | ebXML |
| [RAD-69]     | XDS-I, XCA-I  | Retrieve Imaging Document Set        | `xdsi-rad69`            | SOAP/HTTP(S)  | ebXML |
| [RAD-75]     | XCA-I         | Cross-Gateway Retrieve Imaging Document Set | `xcai-rad75`     | SOAP/HTTP(S)  | ebXML |
| [PCC-1]      | QED           | Query for Existing Data              | `qed-pcc1`              | SOAP/HTTP(S)  | HL7v3 |
| [PCC-44]     | QEDm          | Query for Existing Data Mobile       | `qedm-pcc44`            | REST/HTTP(S)  | FHIR |
| [PCD-01], Continua WAN | PCD, Continua | Communicate Patient Care Device (PCD) Data | `pcd-pcd01` | SOAP/HTTP(S) | HL7 v2.6 |
| [CH-PIDD]    | CH:HPD        | Provider Information Delta Download  | `ch-pidd`               | SOAP/HTTP(S)  | Custom/DSMLv2 |
| [CH-PPQ-1]   | CH:PPQ        | Privacy Policy Feed                  | `ch-ppq1`               | SOAP/HTTP(S)  | Custom/XACML 2.0  |
| [CH-PPQ-2]   | CH:PPQ        | Privacy Policy Retrieve              | `ch-ppq2`               | SOAP/HTTP(S)  | XACML 2.0 |
| [All] MLLP-based | n/a       | Accept requests for multiple MLLP-based transactions through a single TCP port | `mllp-dispatch` | MLLP(S) | HL7v2 |
| [Custom] MLLP-based | n/a    | Accept requests for custom MLLP-based transactions | `mllp` | MLLP(S) | HL7v2 |

[ITI-8]: iti8/
[ITI-9]: iti9/
[ITI-10]: iti10/
[ITI-18]: iti18/
[ITI-21]: iti21/
[ITI-22]: iti22/
[ITI-30]: iti30/
[ITI-31]: iti31/
[ITI-38]: iti38/
[ITI-39]: iti39/
[ITI-41]: iti41/
[ITI-42]: iti42/
[ITI-43]: iti43/
[ITI-44]: iti44/
[ITI-45]: iti45/
[ITI-46]: iti46/
[ITI-47]: iti47/
[ITI-51]: iti51/
[ITI-55]: iti55/
[ITI-56]: iti56/
[ITI-57]: iti57/
[ITI-58]: iti58/
[ITI-59]: iti59/
[ITI-61]: iti61/
[ITI-62]: iti62/
[ITI-63]: iti63/
[ITI-64]: iti64/
[ITI-65]: iti65/
[ITI-66]: iti66/
[ITI-67]: iti67/
[ITI-68]: iti68/
[ITI-78]: iti78/
[ITI-81]: iti81/
[ITI-83]: iti83/
[ITI-86]: iti86/
[RMU ITI-X1]: rmu-X1/
[RAD-69]: rad69/
[RAD-75]: rad75/
[PCC-1]: pcc1/
[PCC-44]: pcc44/
[PCD-01]: pcd01/
[CH-PIDD]: ch-pidd/
[CH-PPQ-1]: ch-ppq1/
[CH-PPQ-2]: ch-ppq2/
[All]: ../mllpDispatch/
[Custom]: ../mllpCustom/
[IHE]: https://www.ihe.net