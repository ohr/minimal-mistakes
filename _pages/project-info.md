---
title: Project Info
layout: single
permalink: /project-info/
sidebar:
  - image: info.jpg
    text: Information about the IPF project
---

## About

The Open eHealth Integration Platform (IPF) provides interfaces for health-care related integration solutions.
An prominent example of an healthcare-related use case of IPF is the implementation of interfaces for transactions specified
in Integrating the Healthcare Enterprise ([IHE][ihe]) profiles.

IPF is built upon and extends the [Apache Camel](https://camel.apache.org) routing and mediation engine. 
It comes with comprehensive support for message processing and connecting
systems in the eHealth domain. IPF provides domain-specific languages (DSLs) for implementing
[Enterprise Integration Patterns](https://www.enterpriseintegrationpatterns.com/)
in general-purpose as well as healthcare-specific integration solutions.

## Development

[Contribute][development] by reporting issues, suggesting new features, or forking the
Git repository on [GitHub][ipf-github] and provide some good pull requests!

## License

IPF code is Open Source and licensed under [Apache license][apache-license].

## Update Instructions

If you are using previous versions of IPF and want to update:

* IPF 3.5.x comes with some changes that must be considered when upgrading from IPF 3.4.x. Read the [3.5 Update Instructions] for how to update from IPF 3.4.x
* IPF 3.4.x comes with some changes that must be considered when upgrading from IPF 3.2.x or IPF 3.3.x to IPF 3.4.x Read the [3.4 Update Instructions] for how to update from IPF 3.1.x
* IPF 3.2.x comes with some changes that must be considered when upgrading from IPF 3.1.x to IPF 3.2.x Read the [3.2 Update Instructions] for how to update from IPF 3.1.x
* IPF 3.1.x introduces a few minor incompatibilities compared to IPF 3.0.x due to having less mandatory dependencies on the Spring framework. Read the [3.1 Update Instructions] for how to update from IPF 3.0.x
* IPF 3.0.x is not backwards-compatible with IPF 2.x. Read the [Migration Instructions] for how to migrate a IPF 2.x-based integration solution.

## Removed functionality

* As XDS.a transactions have been retired by IHE, all functionality related with ITI-14, ITI-15, ITI-16 and ITI-17
have been removed. This includes the ebXML/ebRS 2.x model classes.
{: .notice--warning}
 
## Added modules

IPF 3.5.1 added the modules listed below:

 * `ipf-commons-ihe-xacml20`
 * `ipf-platform-camel-ihe-xacml20`
 
## Deprecated modules

IPF 3.5.1 deprecates the following modules:

 * `ipf-commons-ihe-fhir-dstu2`
 * `ipf-platform-camel-ihe-fhir-dstu2`

These modules will be removed as soon as FHIR r4 support has been included.

## Javadocs

The javadocs can be obtained [here](apidocs/index.html).





[apache-license]: https://www.apache.org/licenses/LICENSE-2.0
[development]: development.html
[ipf-github]: https://github.com/oehf/ipf
[ihe]: https://www.ihe.net
[Migration Instructions]: migration.html
[3.1 Update Instructions]: migration-3.1.html
[3.2 Update Instructions]: migration-3.2.html
[3.4 Update Instructions]: migration-3.4.html
[3.5 Update Instructions]: migration-3.5.html