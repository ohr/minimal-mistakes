---
title: IPF 3.1 Migration Guide
layout: single
permalink: /docs/migration-3.1/
toc: true
toc_icon: align-left  
toc_sticky: true
---

IPF 3.1 comes with some backward-incompatible changes, mostly due to reducing mandatory dependencies to
the Spring framework and some internal refactorings in the IHE-related classes. 


## Spring dependencies

IPF 3.1 encapsulates most Spring dependencies in the new ```ipf-commons-spring``` module.
When you use IPF with Spring, the required Maven dependency is:

```xml
    <dependency>
        <groupId>org.openehealth.ipf.commons</groupId>
        <artifactId>ipf-commons-spring</artifactId>
        <version>${ipf.version}</version>
    </dependency>
```

The only other modules that directly depend on Spring are ```ipf-commons-flow``` and ```ipf-platform-camel-flow```.
All other modules do not depend on Spring anymore. If you so far benefitted from transitive Spring dependencies,
you may need to add these dependencies in your modules.

### Mapping Service

[`org.openehealth.ipf.commons.map.BidiMappingService`](apidocs/org/openehealth/ipf/commons/map/BidiMappingService.html) 
cannot be configured with Spring's `Resource` objects anymore. With Spring, use 
[`org.openehealth.ipf.commons.spring.map.SpringBidiMappingService`](apidocs/org/openehealth/ipf/commons/spring/map/SpringBidiMappingService.html)  
instead and call the new `setMappingResource` or `setMappingResources` methods. The new class is located in the `ipf-commons-spring` module.
See [Mapping Service] for details.

## MLLP custom interceptors

The MLLP-based IHE endpoints offer the possibility to add custom interceptors. The *interceptors* parameter
has been removed due to its dependency on the Spring framework. as of IPF 3.1, use the *interceptorFactories*
parameter instead. See [Custom Interceptors] for details.

## Logging Interceptors

The interceptors logging the payload of IHE MLLP or Web Service transactions use the Spring Expression language
for determining the target file name they write into. The dependency to this library has been made optional.
See [MLLP Payload Logging] and [WS Payload Logging] for details.

## XDS

[`org.openehealth.ipf.commons.ihe.xds.core.metadata.Document`](apidocs/org/openehealth/ipf/commons/ihe/xds/core/metadata/Document.html) 
offered type conversion by directly depending on Spring's `ConversionService`. 
The new interface [`org.openehealth.ipf.commons.core.config.TypeConverter`](apidocs/org/openehealth/ipf/commons/core/config/TypeConverter.html) 
lets you now choose the type converter implementation. 
The only implementation provided by IPF is [`org.openehealth.ipf.commons.spring.core.config.SpringTypeConverter`](apidocs/org/openehealth/ipf/commons/core/config/SpringTypeConverter.html), 
located in the new `ipf-commons-spring` module, which realizes the previous behavior.

## ATNA Auditing and IHE components

All IHE components (MLLP-based, SOAP-based, FHIR-based) have been refactored to use a common source of ATNA
auditing strategies, which all now all located in the ipf-commons-ihe-XXX modules. During this refactoring,
most classes have been touched, mostly changing their type parameters and moving back and forth the one or
other method.
This change will not be visible for 'normal' users of the IHE components provided by IPF. If you, however,
created your own components, endpoints, audit strategies, etc. based on the respective abstract IPF classes, 
you will need to reproduce these refactorings in your own code. Please inspect the standard IHE transaction
classes in IPF for what needs to be changed.


[Mapping Service]: {{ site.baseurl }}{% link _pages/mapping.md %}
[Custom Interceptors]: {{ site.baseurl }}{% link _pages/ihe/mllp/hl7v2InterceptorChain.md %}
[MLLP Payload Logging]: {{ site.baseurl }}{% link _pages/ihe/mllp/hl7v2PayloadLogging.md %}
[WS Payload Logging]: {{ site.baseurl }}{% link _pages/ihe/ws/wsPayloadLogging.md %}