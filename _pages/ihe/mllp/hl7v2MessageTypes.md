---
title: Handling of data types and exceptions in MLLP-based IPF IHE components
layout: single
permalink: /docs/ihe/hl7v2MessageTypes/
toc: true
toc_icon: align-left
toc_sticky: true
---

The IHE Camel components provided by IPF accept and produced a fixed set of message types. These types differ
depending on whether an endpoint acts as producer or consumer.

## Consumer-side requests

Consumer-side requests are automatically unmarshalled, i.e. the incoming message stream sent by the client 
is transformed into a HAPI Message object. 
When unmarshalling fails, an HL7 NAK response will be automatically generated and passed back to the sender.

Unmarshalling may also fail already earlier during reception of the MLLP byte stream (particularly when invalid 
characters are encountered due to misconfigured character set). You have the following two possiblities to
handle these kind of errors:

* add a parameter `consumer.exceptionHandler=#myExceptionHandler` where the configured exception handler must
implement the interface `org.apache.camel.spi.ExceptionHandler`.
* add a parameter `consumer.bridgeErrorhandler=true` which causes the exception handlers in the consumer route
to be called

In both cases, you should populate the message body with a proper HL7 NAK response to be returned to the sender.

## Producer-side responses

Producer-side responses are automatically unmarshalled, i.e. the incoming message stream returned by the server 
is transformed into a HAPI Message object. When unmarshalling fails, an exception will be thrown.

## Consumer-side responses

Consumer-side responses are accepted in a number of data types that will be converted into a HL7 message stream
to be returned to the client:

* HAPI [`ca.uhn.hl7v2.model.Message`](https://hapifhir.github.io/hapi-hl7v2/base/apidocs/ca/uhn/hl7v2/model/Message.html)
* `String`
* `byte[]`
* `java.nio.ByteBuffer`
* `java.io.InputStream`
* `java.io.File`
* [`org.apache.camel.component.file.GenericFile<File>`](https://camel.apache.org/maven/current/camel-core/apidocs/org/apache/camel/component/file/GenericFile.html)
* [`org.apache.camel.WrappedFile<File>`](https://camel.apache.org/maven/current/camel-core/apidocs/org/apache/camel/WrappedFile.html)

In addition, the message body can contain an `Exception` instance, which will be transformed into a NAK response. 
Any exceptions thrown in the route that are not handled otherwise will lead to NAK responses as well.
When neither the data type of the response message is supported nor an exception has been thrown in the route, the message header
`org.openehealth.ipf.platform.camel.ihe.mllp.core.MllpComponent.ACK_TYPE_CODE_HEADER` will be taken into consideration.

When the value of this header belongs to the enumeration type [`ca.uhn.hl7v2.AcknowledgmentCode`](https://hapifhir.github.io/hapi-hl7v2/base/apidocs/ca/uhn/hl7v2/AcknowledgmentCode.html), an acknowledgement will be
automatically generated and sent back to the client — a positive one for `AcknowledgmentCode.AA`,
a negative one (NAK) for `AcknowledgmentCode.AE` and `AcknowledgmentCode.AR`.

When even this header is not set or when its value is not of desired type, the consumer route fails.

## Producer-side requests

Producer-side requests are accepted in a number of data types that will be converted into a HL7 message stream
to be sent to the server:

* `ca.uhn.hl7v2.model.Message` (HAPI)
* `String`
* `byte[]`
* `java.nio.ByteBuffer`
* `java.io.InputStream`
* `java.io.File`
* `org.apache.camel.component.file.GenericFile<File>`
* `org.apache.camel.WrappedFile<File>`

