---
title: Groovy Features for IPF supporting Apache Camel
layout: single
permalink: /docs/groovy/extensions/
toc: true
toc_icon: align-left
---


All Camel DSL extensions require that the Camel routes are written using the Groovy programming language.

## Groovy Camel extensions

In previous IPF versions, Camel DSL elements have been extended to accept Groovy closures as arguments.
As of IPF 3, these extensions are provided directly by the [camel-groovy] DSL of Apache Camel.
The corresponding IPF extensions have been deprecated and moved to the `ipf-platform-camel-core-legacy` module.

See [here]({{ site.baseurl }}{% link _pages/groovy/camelExtensions.md %}) for details.

## Complex DSL extensions

Complex DSL extensions are not complex to use, but they encapsulate enterprise integration patterns that would be anything
but trivial to write using plain Camel.

See [here]({{ site.baseurl }}{% link _pages/groovy/complexExtensions.md %}) for details.

## DSL extensions for IPF module adapters

The `org.openehealth.ipf.commons.core.modules.api` package, which is part of the ipf-commons-core component, defines a
number of interfaces representing typical message processors like Validator, Aggregator or Transmogrifer (a transformer).
The integration of these message processors into IPF routes is done via generic module adapters.
They translate from Camel-specific interfaces to the interfaces and are provided by the `ipf-platform-camel-core` component.

See [here]({{ site.baseurl }}{% link _pages/groovy/moduleAdapters.md %}) for details.

## Other Camel DSL extensions

Some other DSL extensions, mostly for convenience. See [here]({{ site.baseurl }}{% link _pages/groovy/other.md %}) for details.