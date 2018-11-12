---
title: Message types in IPF XDS/XCA/XCF/RAD Components
layout: single
permalink: /docs/ihe/xdsMessageTypes/
classes: wide
---


IPF XDS/XCA Components support both the "raw" ebXML format prescribed by the IHE specification as well as simplified data representation.
See corresponding sections below for details.

## ebXML data model

According to the IHE specification, XDS/XCA/XCF/RAD transactions
use [ebXML 3.0](http://www.oasis-open.org/committees/regrep/documents/3.0/) data models for incoming and outgoing messages.
IPF XDS/XCA components use Java stub classes instead of plain XML payload. All conversions are performed automatically when needed.

Data types for the *request* message of the supported transactions are listed in the table below:

| Transaction              | Request Message Type (`org.openehealth.ipf.commons.ihe.xds.core...`)
|--------------------------|--------------------------------------------------------------------------------------------------
| ITI-18,38,51,63          | [`.stub.ebrs30.query.AdhocQueryRequest`](../apidocs/org/openehealth/ipf/commons/ihe/xds/core/stub/ebrs30/query/AdhocQueryRequest.html)
| ITI-39,43 	           | [`.ebxml.ebxml30.RetrieveDocumentSetRequestType`](../apidocs/org/openehealth/ipf/commons/ihe/xds/core/ebxml/ebxml30/RetrieveDocumentSetRequestType.html)
| ITI-41 	               | [`.ebxml.ebxml30.ProvideAndRegisterDocumentSetRequestType`](../apidocs/org/openehealth/ipf/commons/ihe/xds/core/ebxml/ebxml30/ProvideAndRegisterDocumentSetRequestType.html)
| ITI-42,57,61,92          | [`.stub.ebrs30.lcm.SubmitObjectsRequest`](../apidocs/org/openehealth/ipf/commons/ihe/xds/core/stub/ebrs30/lcm/SubmitObjectsRequest.html)
| ITI-62 	               | [`.stub.ebrs30.lcm.RemoveObjectsRequest`](../apidocs/org/openehealth/ipf/commons/ihe/xds/core/stub/ebrs30/lcm/RemoveObjectsRequest.html)
| ITI-86                   | [`.ebxml.ebxml30.RemoveDocumentsRequestType`](../apidocs/org/openehealth/ipf/commons/ihe/xds/core/ebxml/ebxml30/RemoveDocumentsRequestType.html)          
| RAD-69,75 	           | [`.ebxml.ebxml30.RetrieveImagingDocumentSetRequestType`](../apidocs/org/openehealth/ipf/commons/ihe/xds/core/ebxml/ebxml30/RetrieveImagingDocumentSetRequestType.html)


Data types for the *response* message of the supported transactions are listed in the table below:

| Transaction                       | Response Message Type (`org.openehealth.ipf.commons.ihe.xds.core...`)
|-----------------------------------|--------------------------------------------------------------------------------------------------
| ITI-18,38,51,63                   | [`.stub.ebrs30.query.AdhocQueryResponse`](../apidocs/org/openehealth/ipf/commons/ihe/xds/core/stub/ebrs30/query/AdhocQueryResponse.html)
| ITI-39,43; RAD-69,75              | [`.ebxml.ebxml30.RetrieveDocumentSetResponseType`](../apidocs/org/openehealth/ipf/commons/ihe/xds/core/ebxml/ebxml30/RetrieveDocumentSetResponseType.html)
| ITI-41,42,57,61,62,86,92          | [`.stub.ebrs30.rs.RegistryResponseType`](../apidocs/org/openehealth/ipf/commons/ihe/xds/core/stub/ebrs30/rs/RegistryResponseType.html)


## Simplified Data Model

Besides the support of raw ebXML messages, the IPF offers a model that abstracts from the ebXML representation of the data
and is preferable for the user due to its close relation to the application domain.

Data types for the *request* message of the supported transactions are listed in the table below:

| Transaction              | Request Message Type (`org.openehealth.ipf.commons.ihe.xds.core.requests...`)
|--------------------------|--------------------------------------------------------------------------------------------------
| ITI-42,57,61,92          | [`.RegisterDocumentSet`](../apidocs/org/openehealth/ipf/commons/ihe/xds/core/requests/RegisterDocumentSet.html)
| ITI-41                   | [`.ProvideAndRegisterDocumentSet`](../apidocs/org/openehealth/ipf/commons/ihe/xds/core/requests/ProvideAndRegisterDocumentSet.html)
| ITI-18,38,51,63          | [`.QueryRegistry`](../apidocs/org/openehealth/ipf/commons/ihe/xds/core/requests/QueryRegistry.html)
| ITI-39,43 	           | [`.RetrieveDocumentSet`](../apidocs/org/openehealth/ipf/commons/ihe/xds/core/requests/RetrieveDocumentSet.html)
| ITI-62 	               | [`.RemoveMetadata`](../apidocs/org/openehealth/ipf/commons/ihe/xds/core/requests/RemoveMetadata.html)
| ITI-86       	           | [`.RemoveDocuments`](../apidocs/org/openehealth/ipf/commons/ihe/xds/core/requests/RemoveDocuments.html)
| RAD-69,75 	           | [`.RetrieveImagingDocumentSet`](../apidocs/org/openehealth/ipf/commons/ihe/xds/core/requests/RetrieveImagingDocumentSet.html)

Data types for the *response* message of the supported transactions are listed in the table below:

| Transaction                          | Response Message Type (`org.openehealth.ipf.commons.ihe.xds.core.responses...`)
|--------------------------------------|--------------------------------------------------------------------------------------------------
| ITI-41,42,57,61,62,86,92             | [`.Response`](../apidocs/org/openehealth/ipf/commons/ihe/xds/core/responses/Response.html)
| ITI-18,38,51,63                      | [`.QueryResponse`](../apidocs/org/openehealth/ipf/commons/ihe/xds/core/responses/QueryResponse.html)
| ITI-39,43; RAD-69,75                 | [`.RetrievedDocumentSet`](../staging/apidocs/org/openehealth/ipf/commons/ihe/xds/core/responses/RetrievedDocumentSet.html)

Some XDS model classes from the package `org.openehealth.ipf.commons.ihe.xds.core.metadata` wrap [HAPI] objects:

| Class Name         | HL7 v2 data holder type
|--------------------|------------------------------
| Address            | XAD
| AssigningAuthority | Holder&lt;HD&gt;
| Code               | CE
| Identifiable       | CX
| Organization       | XON
| Person             | XCN
| ReferenceId        | CX
| Telecom            | XTN
| XcnName            | XCN
| XpnName            | XPN

These classes have constructors which take [HAPI] objects as parameters, thus providing the possibility to create XDS objects
directly from HL7v2 ones. The wrapped [HAPI] objects can be accessed via `.getHapiObject()` methods of the XDS model class instances
(for `AssigningAuthority`: `.getHapiObject().getInternal()`).

The static methods `render(object)` and `rawRender(object)` of the class `org.openehealth.ipf.commons.ihe.xds.core.metadata.Hl7v2Based`
can be used for getting an HL7v2 string representation of the given XDS model object.
`render(object)` considers all IHE rules regarding unwanted components (see ITI TF Volume 3 Section 4.1), and, for example,
omits display name in CE codes. `rawRender(object)` renders all components, even if they do not have any meaning in XDS context.

The static method `parse()` performs the opposite conversion — from an Hl7 v2 string to an XDS simplified object model.

The classes `XcnName` and `XpnName` both implement the interface `Name`, are compatible in regard to the method `.equals()`
and fully interchangeable in all situations.


## Transformation between the models

Transformation between the ebXML and the simplified data models can be performed by means of
[Camel type converters](https://camel.apache.org/type-converter.html) provided by the IPF, as outlined in the following example.

Type converters are applied by either using the `convertBodyTo(Class)` processor, or by converting the message body on the fly,
e.g. using `exchange.getIn().getBody(Class)`.

Although the route below is written in Groovy, type conversion does not require the Groovy programming language.

```groovy

    import static org.openehealth.ipf.commons.ihe.xds.core.responses.Status.*

    import org.apache.camel.spring.SpringRouteBuilder
    import org.openehealth.ipf.commons.ihe.xds.core.requests.QueryRegistry
    import org.openehealth.ipf.commons.ihe.xds.core.requests.query.FindDocumentsQuery
    import org.openehealth.ipf.commons.ihe.xds.core.responses.QueryResponse
    import org.openehealth.ipf.commons.ihe.xds.core.metadata.ObjectReference
    import org.openehealth.ipf.platform.camel.core.util.Exchanges

    class RouteBuilder extends SpringRouteBuilder {
    
        @Override
        void configure() throws Exception {
            from("xds-iti18:myIti18Service")
                .convertBodyTo(QueryRegistry.class)
                .choice()
                    // Return an object reference for a find documents query
                    .when { it.in.body.query instanceof FindDocumentsQuery }
                        .transform {
                            def response = new QueryResponse(SUCCESS)
                            response.references.add(new ObjectReference('document01'))
                            response
                        }
                    // Any other query else is a failure
                    .otherwise()
                        .transform { new QueryResponse(FAILURE) }
        }
    }

```

[HAPI]: https://hapifhir.github.io/hapi-hl7v2