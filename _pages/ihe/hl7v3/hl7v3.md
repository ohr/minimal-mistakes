---
title: HL7v3 based transactions
layout: single
permalink: /docs/ihe/hl7v3/
classes: wide
---

 
IPF adds support for a subset of HL7v3-based profiles by providing Camel components (hiding the 
 implementation details on transport level) and [translators] between the Hl7 v3 and HL7 v2 message models.

While the FHIR transactions in IHE ITI revision 13 (2016/2017) was based on FHIR [DSTU2](https://hl7.org/fhir/DSTU2/index.html),
the transactions have been migrated to [STU3](https://hl7.org/fhir/index.html) as of IHE ITI revision 14 (2017/2018).
{: .notice--info}

The following IHE transactions for FHIR are currently supported:

| Transaction             | Profile          | Description           | IPF Component          |  IPF Module |
|:------------------------|:-----------------|:----------------------|:-----------------------|:------------|
{% for hash in site.data.ihe -%}
  {% assign tx = hash[1] -%}
  {% if tx.format == "HL7v3" -%}
| [{{ tx.transaction }}](../{{ tx.link }}/)  | {{ tx.profile }} | {{ tx.description }}  | `{{ tx.component }}`  | `{{ tx.module }}` |
  {% endif -%}
{% endfor %}

[translators]: {{ site.baseurl }}{% link _pages/ihe/hl7v3/hl7v3Translators.md %}