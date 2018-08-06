---
title: Spring Boot HPD support
layout: single
permalink: /docs/boot-hpd/
classes: wide
---

`ipf-hpd-spring-boot-starter` sets up the infrastructure for transactions of the IHE HPD profile.
 
The dependency on the IPF [Spring Boot] IHE HPD starter module is:

```xml
    <dependency>
        <groupId>org.openehealth.ipf.boot</groupId>
        <artifactId>ipf-hpd-spring-boot-starter</artifactId>
    </dependency>
```

`ipf-hpd-spring-boot-starter` provides the following application properties:

| Property (`ipf.hpd.`)     | Default        | Description                                         |
|---------------------------|----------------|-----------------------------------------------------|
|                           |                |

See [ipf-spring-boot-starter]({{ site.baseurl }}{% link _pages/boot/boot.md %}) and [ipf-atna-spring-boot-starter]({{ site.baseurl }}{% link _pages/boot/boot-atna.md %}) for
additional properties.

This starter module also transitively depends on [cxf-spring-boot-starter-jaxws](https://cxf.apache.org/docs/springboot.html) that sets up the CXF
web service stack including the Camel CXF servlet, so you don't have to care about this anymore.

`cxf-spring-boot-starter-jaxws` provides the following application properties:

| Property (`cxf.`)          | Default                | Description                                         |
|----------------------------|------------------------|-----------------------------------------------------|
| `path`                     | /services              | Path that serves as the base URI for the services
| `servlet.init`             | empty map              | optional servlet init parameters
| `servlet.load-on-startup`  | -1                     | startup order


[Spring Boot]: https://projects.spring.io/spring-boot/
