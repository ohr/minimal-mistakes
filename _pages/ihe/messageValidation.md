---
title: Message Validation in IPF eHealth Components
layout: single
permalink: /docs/ihe/messageValidation/
toc: true
toc_icon: align-left
toc_sticky: true
---

Message validation in IPF eHealth components are implemented by means of validating [Camel processors](https://camel.apache.org/processor.html),
available for incoming and outgoing messages of all implemented transactions. 

Per convention, each of these validating processors throws an
[`org.openehealth.ipf.commons.core.modules.api.ValidationException`](../apidocs/org/openehealth/ipf/commons/core/modules/api/ValidationException.html)
when the validation fails.
This exception contains detailed description of detected validation problem(s).

Instances of validating processors, which can be directly used in a route by means of the [.process()](https://camel.apache.org/routes.html#Routes-Usingacustomprocessor)
DSL element, can be obtained from the corresponding factories, depending on the eHealth integration profile:

| Integration profile                        | Validating processors factory                                              |
|:-------------------------------------------|:---------------------------------------------------------------------------|
| IHE XDS, XDM, XDR, XCA, XCF, RAD, RMD, RMU | `org.openehealth.ipf.platform.camel.ihe.xds.XdsCamelValidators` |
| IHE PIX, PDQ, XAD-PID                      | `org.openehealth.ipf.platform.camel.ihe.mllp.PixPdqCamelValidators` |
| IHE PAM                                    | `org.openehealth.ipf.platform.camel.ihe.mllp.PamCamelValidators` |
| IHE PIXv3, PDQv3, XCPD, QED                | `org.openehealth.ipf.platform.camel.ihe.hl7v3.PixPdqV3CamelValidators` |
| IHE HPD                                    | `org.openehealth.ipf.platform.camel.ihe.hpd.HpdCamelValidators` |
| IHE PCD, Continua WAN                      | `org.openehealth.ipf.platform.camel.ihe.hl7v2ws.Hl7v2WsCamelValidators` |
| Continua HRN                               | `org.openehealth.ipf.platform.camel.ihe.continua.hrn.ContinuaHrnCamelProcessors` |
| IHE FHIR-based profiles                    | `org.openehealth.ipf.platform.camel.ihe.fhir.core.FhirCamelValidators`  |
| Swiss CH:PPQ                               | `org.openehealth.ipf.platform.camel.ihe.xacml20.Xacml20CamelValidators` |

Factory methods of these classes (excluding Continua HRN and FHIR) obey the following naming convention:

```java
    public static Processor iti<transaction number><direction>Validator();
    public static Processor rad<transaction number><direction>Validator();   
    public static Processor pcc<transaction number><direction>Validator();   // for QED PCC-1 transaction
    public static Processor pcd<transaction number><direction>Validator();   // for PCD-01 transaction
    public static Processor continuaWan<direction>Validator();               // for Continua WAN
    ...
```

where

* `<transaction>` number is the number/id of IHE transaction from the profile supported by the given factory
* `<direction>` is either "Request" or "Response"

For FHIR transaction, only the self-detecting message validation is defined (see below)

## Example

An example of using validating processors is given below:

```java
    import static org.openehealth.ipf.platform.camel.ihe.hl7v3.PixPdqV3CamelValidators.*;
    import org.openehealth.ipf.commons.core.modules.api.ValidationException;
    import org.apache.camel.spring.SpringRouteBuilder;

    public class MyRouteBuilder extends SpringRouteBuilder {
        @Override
        public void configure() throws Exception {
            from("pdqv3-iti47:iti47Service")
                .onException(ValidationException.class)
                    .maximumRedeliveries(0)
                    ...                               // handle validation failure appropriately
                    .end()
                .process(iti47RequestValidator())     // validate received PDQ v3 request message
                ...                                   // prepare response
                .process(iti47ResponseValidator());   // validate prepared PDQ v3 response message
        }
    }
```


## Self-detecting message validation in MLLP consumer routes

The consumer side of IPF MLLP transaction endpoints receiving HL7v2 messages from clients is aware of the
IHE transaction context. The consumer endpoint can therefore initialize an [`HapiContext`](https://hapifhir.github.io/hapi-hl7v2/base/apidocs/ca/uhn/hl7v2/HapiContext.html)
that is passed into the route by the [HL7 Codec](codec.html). This context also contains the appropriate 
[`ValidationContext`](https://hapifhir.github.io/hapi-hl7v2/base/apidocs/ca/uhn/hl7v2/validation/ValidationContext.html) that is initialized with the HL7 conformance
profile matching the transaction.
As a result, no explicit validation processor must be provided anymore:

```java
    import static org.openehealth.ipf.platform.camel.ihe.mllp.PixPdqCamelValidators.*;
    import org.openehealth.ipf.commons.core.modules.api.ValidationException;
    import org.apache.camel.builder.RouteBuilder;

    public class MyRouteBuilder extends RouteBuilder {
        @Override
        public void configure() throws Exception {
            from("pix-iti8:0.0.0.0:3700")
                .onException(ValidationException.class)
                    .maximumRedeliveries(0)
                    ...                               // handle validation failure appropriately
                    .end()
                .process(itiValidator())              // validate received PIX Feed request message
                ...                                   // prepare response
                .process(itiValidator());             // validate prepared PIX Feed acknowledgement
        }
    }
```


## Self-detecting message validation in FHIR consumer routes

The consumer side of IPF FHIR transaction endpoints receiving FHIR resources from clients is aware of the
IHE transaction context. The consumer endpoint can therefore initialize an [`FhirContext`](http://hapifhir.io/apidocs/ca/uhn/fhir/context/FhirContext.html)
that is passed into the route.
As a result, no explicit validation processor must be provided anymore:

```java
    import static org.openehealth.ipf.platform.camel.ihe.fhir.core.FhirCamelValidators.*;

    public class MyRouteBuilder extends RouteBuilder {
        @Override
        public void configure() throws Exception {
            from("mhd-iti65:stub?audit=true")
                .errorHandler(noErrorHandler())
                .setHeader(VALIDATION_MODE, constant(SCHEMA | MODEL))
                .process(itiRequestValidator())       // validate received FHIR resource
                ...                                   // prepare response
        }
    }
```

The depth of validation is determined by the integer constant contained in the `FhirCamelValidators.VALIDATION_MODE` header. In the
example above, schema and model validation are executed.


## Disabling message validation

It is possible to switch off validation by means of a Camel message header. 
When there is input message header named `org.openehealth.ipf.platform.camel.core.adapter.ValidatorAdapter#NEED_VALIDATION_HEADER_NAME`
and its value equals to `false`, the validation step will be skipped. This gives the possibility to validate conditionally
based of user-defined properties and/or make it configurable e.g. via JMX.

Please be aware that the aforementioned Camel message header will remain untouched and deactivate subsequent validation steps as well.