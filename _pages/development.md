---
title: Development
layout: single
permalink: /development/
sidebar:
  - image: assets/images/development.jpg
---

### Building

IPF requires Java 8 for both compile time and runtime.
IPF does not yet support Java 9+.

IPF builds using Maven 3.5.x. IPF is available at [Maven Central], so no custom repositories need to
be added to the `settings.xml` configuration file.

Before building, adjust `MAVEN_OPTS` to assign Maven more heap space.

```
    set MAVEN_OPTS=-Xmx1024m
    mvn clean install
```

### Documentation

In order to generate the site documentation, Java stubs from Groovy and Lombok
sources must be generated for proper Javadoc creation during the `site` phase.

```
    set MAVEN_OPTS=-Xmx1024m
    mvn -Pgenerate-stubs generate-sources 
    mvn site (-DskipTests)
    mvn site:stage
```

### Sources

IPF uses [git](https://git-scm.com/) for source code management. The IPF git repository is located at
[https://github.com/oehf/ipf](https://github.com/oehf/ipf).

Additionally, there are the following support projects:

* `ipf-gazelle`, which provides conformance profiles for HL7v2 based IHE transactions.
It may be released independently and is located at [https://github.com/oehf/ipf-gazelle](https://github.com/oehf/ipf-gazelle).
* `ipf-oht-atna`, which provides infrastructure for IHE audit trails and node authentication via TLS.
It may be released independently and is located at [https://github.com/oehf/ipf-oht-atna](https://github.com/oehf/ipf-oht-atna).

**Watch out!** `ipf-oht-atna` libraries have been deprecated, and IPF has removed all dependencies targeting at `ipf-oht-atna`.
{: .notice--warning}

### Continuous Integration

IPF is continuously built on [Travis](https://travis-ci.org/oehf). Snapshot artifacts are uploaded to the 
[Sonatype](https://oss.sonatype.org/content/repositories/snapshots/org/openehealth/ipf/) snapshot repository.

### Issue Tracking

Issue tracking is done in github. For current issues check [https://github.com/oehf/ipf/issues](https://github.com/oehf/ipf/issues).

### IDE

IPF depends on Maven, [Groovy](https://www.groovy-lang.org/), [Kotlin](https://kotlinlang.org/) and [Lombok](https://projectlombok.org/).
Depending on the choice of your IDE, you may need to install corresponding plugins.


[Maven Central]: https://search.maven.org