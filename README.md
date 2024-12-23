# Snyk's SBOM generation feature CLI demos

A collection of different examples using Snyk's SBOM generation feature, you must use a minimum of CLI version 1.1071.0 for these demos and have an active internet connection

## Prerequisites

- Install the Snyk CLI using the instructions here https://docs.snyk.io/snyk-cli/install-the-snyk-cli
- AUTH with Snyk CLI https://docs.snyk.io/snyk-cli/commands/auth
- Verify you are ready to use the CLI as shown below. Ensure you are running version "**1.1071.0**" or later

```shell
$ snyk --version
1.1266.0
```

- The demos will use the following repo if you wish to follow along clone the repo https://github.com/papicella/snyk-boot-web as shown below 

```shell
$ git clone https://github.com/papicella/snyk-boot-web
```

- All demo will use JQ for formatting the output you don't need this but it's handy to install this as well https://jqlang.github.io/jq/download/
- Have either curl or HTTPie installed

```shell
$ http --version
3.2.2

$ which curl
/usr/bin/curl
```

- Finally, if you have multiple organizations, you can set a default from the CLI using:

_Note: You can also use --org=<orgslugname>. The ORG_ID works in both the CLI and the API. The organization slug name works in the CLI, but not in the API_

```shell
$ snyk config set org=<ORG_ID>
```

## Demos

## Snyk SBOM Open-Source demos

An SBOM can be generated for all supported Open Source package managers as well as unmanaged software projects

- First let's generate our first SBOM, in this example we reference the maven pom file, this will ensure we generate a SBOM for the project we are interested in 

_Note: You can also set the format to any of these --format=<cyclonedx1.4+json|cyclonedx1.4+xml|spdx2.3+json>_

```shell
$ snyk sbom --format=cyclonedx1.4+json --file=pom.xml 
{
  "$schema": "http://cyclonedx.org/schema/bom-1.4.schema.json",
  "bomFormat": "CycloneDX",
  "specVersion": "1.4",
  "version": 1,
  "metadata": {
    "timestamp": "2024-01-01T23:27:15Z",
    "tools": [
      {
        "vendor": "Snyk",
        "name": "snyk-cli",
        "version": "1.1266.0"
      },
      {
        "vendor": "Snyk",
        "name": "SBOM Export API",
        "version": "v1.49.0"
      }
    ],
    "component": {
      "bom-ref": "1-com.example:snyk-boot-web@0.0.1-SNAPSHOT",
      "type": "application",
      "name": "com.example:snyk-boot-web",
      "version": "0.0.1-SNAPSHOT",
      "purl": "pkg:maven/com.example/snyk-boot-web@0.0.1-SNAPSHOT"
    }
  },
  "components": [
    {
      "bom-ref": "2-org.springframework.boot:spring-boot-actuator@2.3.10.RELEASE",
      "type": "library",
      "name": "org.springframework.boot:spring-boot-actuator",
      "version": "2.3.10.RELEASE",
      "purl": "pkg:maven/org.springframework.boot/spring-boot-actuator@2.3.10.RELEASE"
    },

    ....
```

- Given we using a JSON output format we can save the contents of the output to a file as follows

```shell
$ snyk sbom --format=cyclonedx1.4+json --file=pom.xml --json-file-output=apples.json
```

- Now lets use "sbom test" to test the SBOM JSON itself

```shell
$ snyk sbom test --experimental --file=apples.json

Testing apples.json


Open issues:

× [LOW] Information Exposure
  Introduced through: pkg:maven/org.apache.tomcat.embed/tomcat-embed-core@9.0.45
  URL: https://security.snyk.io/vuln/SNYK-JAVA-ORGAPACHETOMCATEMBED-3035793

× [LOW] HTTP Request Smuggling
  Introduced through: pkg:maven/org.apache.tomcat.embed/tomcat-embed-core@9.0.45
  URL: https://security.snyk.io/vuln/SNYK-JAVA-ORGAPACHETOMCATEMBED-3097829

× [LOW] Improper Handling of Case Sensitivity
  Introduced through: pkg:maven/org.springframework/spring-context@5.2.14.RELEASE
  URL: https://security.snyk.io/vuln/SNYK-JAVA-ORGSPRINGFRAMEWORK-2689634

× [LOW] Improper Handling of Case Sensitivity
  Introduced through: pkg:maven/org.springframework/spring-context@5.2.14.RELEASE
  URL: https://security.snyk.io/vuln/SNYK-JAVA-ORGSPRINGFRAMEWORK-8230364

× [LOW] Improper Handling of Case Sensitivity
  Introduced through: pkg:maven/org.springframework/spring-core@5.2.14.RELEASE
  URL: https://security.snyk.io/vuln/SNYK-JAVA-ORGSPRINGFRAMEWORK-8230365

× [LOW] Improper Handling of Case Sensitivity
  Introduced through: pkg:maven/org.springframework/spring-web@5.2.14.RELEASE
  URL: https://security.snyk.io/vuln/SNYK-JAVA-ORGSPRINGFRAMEWORK-8230366

× [LOW] Improper Handling of Case Sensitivity
  Introduced through: pkg:maven/org.springframework/spring-webmvc@5.2.14.RELEASE
  URL: https://security.snyk.io/vuln/SNYK-JAVA-ORGSPRINGFRAMEWORK-8230368

× [LOW] Stack-based Buffer Overflow
  Introduced through: pkg:maven/org.yaml/snakeyaml@1.26
  URL: https://security.snyk.io/vuln/SNYK-JAVA-ORGYAML-3016888

× [LOW] Stack-based Buffer Overflow
  Introduced through: pkg:maven/org.yaml/snakeyaml@1.26
  URL: https://security.snyk.io/vuln/SNYK-JAVA-ORGYAML-3016889

× [LOW] Stack-based Buffer Overflow
  Introduced through: pkg:maven/org.yaml/snakeyaml@1.26
  URL: https://security.snyk.io/vuln/SNYK-JAVA-ORGYAML-3113851

× [MEDIUM] Insufficient Hostname Verification
  Introduced through: pkg:maven/ch.qos.logback/logback-core@1.2.3
  URL: https://security.snyk.io/vuln/SNYK-JAVA-CHQOSLOGBACK-1726923

× [MEDIUM] Denial of Service (DoS)
  Introduced through: pkg:maven/com.fasterxml.jackson.core/jackson-databind@2.11.4
  URL: https://security.snyk.io/vuln/SNYK-JAVA-COMFASTERXMLJACKSONCORE-2326698

× [MEDIUM] Denial of Service (DoS)
  Introduced through: pkg:maven/com.fasterxml.jackson.core/jackson-databind@2.11.4
  URL: https://security.snyk.io/vuln/SNYK-JAVA-COMFASTERXMLJACKSONCORE-3038424

× [MEDIUM] Denial of Service (DoS)
  Introduced through: pkg:maven/com.fasterxml.jackson.core/jackson-databind@2.11.4
  URL: https://security.snyk.io/vuln/SNYK-JAVA-COMFASTERXMLJACKSONCORE-3038426

× [MEDIUM] Information Exposure
  Introduced through: pkg:maven/com.h2database/h2@1.4.200
  URL: https://security.snyk.io/vuln/SNYK-JAVA-COMH2DATABASE-3146851

× [MEDIUM] Arbitrary Code Execution
  Introduced through: pkg:maven/org.apache.logging.log4j/log4j-core@2.15.0
  URL: https://security.snyk.io/vuln/SNYK-JAVA-ORGAPACHELOGGINGLOG4J-2327339

× [MEDIUM] Improper Input Validation
  Introduced through: pkg:maven/org.apache.tomcat.embed/tomcat-embed-core@9.0.45
  URL: https://security.snyk.io/vuln/SNYK-JAVA-ORGAPACHETOMCATEMBED-1728265

× [MEDIUM] HTTP Request Smuggling
  Introduced through: pkg:maven/org.apache.tomcat.embed/tomcat-embed-core@9.0.45
  URL: https://security.snyk.io/vuln/SNYK-JAVA-ORGAPACHETOMCATEMBED-1728266

× [MEDIUM] Denial of Service (DoS)
  Introduced through: pkg:maven/org.apache.tomcat.embed/tomcat-embed-core@9.0.45
  URL: https://security.snyk.io/vuln/SNYK-JAVA-ORGAPACHETOMCATEMBED-3326459

× [MEDIUM] Unprotected Transport of Credentials
  Introduced through: pkg:maven/org.apache.tomcat.embed/tomcat-embed-core@9.0.45
  URL: https://security.snyk.io/vuln/SNYK-JAVA-ORGAPACHETOMCATEMBED-3369687

× [MEDIUM] Access Restriction Bypass
  Introduced through: pkg:maven/org.apache.tomcat.embed/tomcat-embed-core@9.0.45
  URL: https://security.snyk.io/vuln/SNYK-JAVA-ORGAPACHETOMCATEMBED-5862028

× [MEDIUM] Improper Input Validation
  Introduced through: pkg:maven/org.apache.tomcat.embed/tomcat-embed-core@9.0.45
  URL: https://security.snyk.io/vuln/SNYK-JAVA-ORGAPACHETOMCATEMBED-5959654

× [MEDIUM] Incomplete Cleanup
  Introduced through: pkg:maven/org.apache.tomcat.embed/tomcat-embed-core@9.0.45
  URL: https://security.snyk.io/vuln/SNYK-JAVA-ORGAPACHETOMCATEMBED-5959972

× [MEDIUM] Privilege Escalation
  Introduced through: pkg:maven/org.springframework/spring-web@5.2.14.RELEASE
  URL: https://security.snyk.io/vuln/SNYK-JAVA-ORGSPRINGFRAMEWORK-1296829

× [MEDIUM] Improper Output Neutralization for Logs
  Introduced through: pkg:maven/org.springframework/spring-core@5.2.14.RELEASE
  URL: https://security.snyk.io/vuln/SNYK-JAVA-ORGSPRINGFRAMEWORK-2329097

× [MEDIUM] Improper Input Validation
  Introduced through: pkg:maven/org.springframework/spring-core@5.2.14.RELEASE
  URL: https://security.snyk.io/vuln/SNYK-JAVA-ORGSPRINGFRAMEWORK-2330878

× [MEDIUM] Denial of Service (DoS)
  Introduced through: pkg:maven/org.springframework/spring-expression@5.2.14.RELEASE
  URL: https://security.snyk.io/vuln/SNYK-JAVA-ORGSPRINGFRAMEWORK-2434828

× [MEDIUM] Denial of Service (DoS)
  Introduced through: pkg:maven/org.springframework/spring-beans@5.2.14.RELEASE
  URL: https://security.snyk.io/vuln/SNYK-JAVA-ORGSPRINGFRAMEWORK-2823313

× [MEDIUM] Allocation of Resources Without Limits or Throttling
  Introduced through: pkg:maven/org.springframework/spring-expression@5.2.14.RELEASE
  URL: https://security.snyk.io/vuln/SNYK-JAVA-ORGSPRINGFRAMEWORK-3369749

× [MEDIUM] Allocation of Resources Without Limits or Throttling
  Introduced through: pkg:maven/org.springframework/spring-expression@5.2.14.RELEASE
  URL: https://security.snyk.io/vuln/SNYK-JAVA-ORGSPRINGFRAMEWORK-5422217

× [MEDIUM] Open Redirect
  Introduced through: pkg:maven/org.springframework/spring-web@5.2.14.RELEASE
  URL: https://security.snyk.io/vuln/SNYK-JAVA-ORGSPRINGFRAMEWORK-6597980

× [MEDIUM] Denial of Service (DoS)
  Introduced through: pkg:maven/org.springframework/spring-web@5.2.14.RELEASE
  URL: https://security.snyk.io/vuln/SNYK-JAVA-ORGSPRINGFRAMEWORK-7687447

× [MEDIUM] Denial of Service (DoS)
  Introduced through: pkg:maven/org.springframework/spring-webmvc@5.2.14.RELEASE
  URL: https://security.snyk.io/vuln/SNYK-JAVA-ORGSPRINGFRAMEWORK-8384234

× [MEDIUM] Denial of Service (DoS)
  Introduced through: pkg:maven/org.springframework.boot/spring-boot-actuator@2.3.10.RELEASE
  URL: https://security.snyk.io/vuln/SNYK-JAVA-ORGSPRINGFRAMEWORKBOOT-6226862

× [MEDIUM] Stack-based Buffer Overflow
  Introduced through: pkg:maven/org.yaml/snakeyaml@1.26
  URL: https://security.snyk.io/vuln/SNYK-JAVA-ORGYAML-3016891

× [MEDIUM] Arbitrary Code Execution
  Introduced through: pkg:maven/org.yaml/snakeyaml@1.26
  URL: https://security.snyk.io/vuln/SNYK-JAVA-ORGYAML-3152153

× [MEDIUM] Dual license: EPL-1.0, LGPL-2.1
  Introduced through: pkg:maven/ch.qos.logback/logback-classic@1.2.3
  URL: https://security.snyk.io/vuln/snyk:lic:maven:ch.qos.logback:logback-classic:(EPL-1.0_OR_LGPL-2.1)

× [MEDIUM] Dual license: EPL-1.0, LGPL-2.1
  Introduced through: pkg:maven/ch.qos.logback/logback-core@1.2.3
  URL: https://security.snyk.io/vuln/snyk:lic:maven:ch.qos.logback:logback-core:(EPL-1.0_OR_LGPL-2.1)

× [MEDIUM] Dual license: MPL-2.0, EPL-1.0
  Introduced through: pkg:maven/com.h2database/h2@1.4.200
  URL: https://security.snyk.io/vuln/snyk:lic:maven:com.h2database:h2:(MPL-2.0_OR_EPL-1.0)

× [HIGH] Denial of Service (DoS)
  Introduced through: pkg:maven/ch.qos.logback/logback-classic@1.2.3
  URL: https://security.snyk.io/vuln/SNYK-JAVA-CHQOSLOGBACK-6094942

× [HIGH] Denial of Service (DoS)
  Introduced through: pkg:maven/ch.qos.logback/logback-core@1.2.3
  URL: https://security.snyk.io/vuln/SNYK-JAVA-CHQOSLOGBACK-6094943

× [HIGH] Uncontrolled Resource Consumption ('Resource Exhaustion')
  Introduced through: pkg:maven/ch.qos.logback/logback-classic@1.2.3
  URL: https://security.snyk.io/vuln/SNYK-JAVA-CHQOSLOGBACK-6097492

× [HIGH] Uncontrolled Resource Consumption ('Resource Exhaustion')
  Introduced through: pkg:maven/ch.qos.logback/logback-core@1.2.3
  URL: https://security.snyk.io/vuln/SNYK-JAVA-CHQOSLOGBACK-6097493

× [HIGH] Denial of Service (DoS)
  Introduced through: pkg:maven/com.fasterxml.jackson.core/jackson-databind@2.11.4
  URL: https://security.snyk.io/vuln/SNYK-JAVA-COMFASTERXMLJACKSONCORE-2421244

× [HIGH] Denial of Service (DoS)
  Introduced through: pkg:maven/com.fasterxml.jackson.core/jackson-core@2.11.4
  URL: https://security.snyk.io/vuln/SNYK-JAVA-COMFASTERXMLJACKSONCORE-7569538

× [HIGH] XML External Entity (XXE) Injection
  Introduced through: pkg:maven/com.h2database/h2@1.4.200
  URL: https://security.snyk.io/vuln/SNYK-JAVA-COMH2DATABASE-1769238

× [HIGH] Remote Code Execution (RCE)
  Introduced through: pkg:maven/com.h2database/h2@1.4.200
  URL: https://security.snyk.io/vuln/SNYK-JAVA-COMH2DATABASE-2331071

× [HIGH] Remote Code Execution (RCE)
  Introduced through: pkg:maven/com.h2database/h2@1.4.200
  URL: https://security.snyk.io/vuln/SNYK-JAVA-COMH2DATABASE-31685

× [HIGH] Denial of Service (DoS)
  Introduced through: pkg:maven/org.apache.logging.log4j/log4j-core@2.15.0
  URL: https://security.snyk.io/vuln/SNYK-JAVA-ORGAPACHELOGGINGLOG4J-2321524

× [HIGH] Privilege Escalation
  Introduced through: pkg:maven/org.apache.tomcat.embed/tomcat-embed-core@9.0.45
  URL: https://security.snyk.io/vuln/SNYK-JAVA-ORGAPACHETOMCATEMBED-2414084

× [HIGH] Improper Input Validation
  Introduced through: pkg:maven/org.apache.tomcat.embed/tomcat-embed-core@9.0.45
  URL: https://security.snyk.io/vuln/SNYK-JAVA-ORGAPACHETOMCATEMBED-3225086

× [HIGH] Denial of Service (DoS)
  Introduced through: pkg:maven/org.apache.tomcat.embed/tomcat-embed-core@9.0.45
  URL: https://security.snyk.io/vuln/SNYK-JAVA-ORGAPACHETOMCATEMBED-5953331

× [HIGH] Improper Input Validation
  Introduced through: pkg:maven/org.apache.tomcat.embed/tomcat-embed-core@9.0.45
  URL: https://security.snyk.io/vuln/SNYK-JAVA-ORGAPACHETOMCATEMBED-6092281

× [HIGH] Denial of Service (DoS)
  Introduced through: pkg:maven/org.apache.tomcat.embed/tomcat-embed-core@9.0.45
  URL: https://security.snyk.io/vuln/SNYK-JAVA-ORGAPACHETOMCATEMBED-6435948

× [HIGH] Denial of Service (DoS)
  Introduced through: pkg:maven/org.apache.tomcat.embed/tomcat-embed-websocket@9.0.45
  URL: https://security.snyk.io/vuln/SNYK-JAVA-ORGAPACHETOMCATEMBED-6435950

× [HIGH] Insufficient Session Expiration
  Introduced through: pkg:maven/org.apache.tomcat.embed/tomcat-embed-core@9.0.45
  URL: https://security.snyk.io/vuln/SNYK-JAVA-ORGAPACHETOMCATEMBED-7430175

× [HIGH] Allocation of Resources Without Limits or Throttling
  Introduced through: pkg:maven/org.apache.tomcat.embed/tomcat-embed-core@9.0.45
  URL: https://security.snyk.io/vuln/SNYK-JAVA-ORGAPACHETOMCATEMBED-8073090

× [HIGH] Improper Input Validation
  Introduced through: pkg:maven/org.glassfish/jakarta.el@3.0.3
  URL: https://security.snyk.io/vuln/SNYK-JAVA-ORGGLASSFISH-1297098

× [HIGH] Open Redirect
  Introduced through: pkg:maven/org.springframework/spring-web@5.2.14.RELEASE
  URL: https://security.snyk.io/vuln/SNYK-JAVA-ORGSPRINGFRAMEWORK-6261586

× [HIGH] Open Redirect
  Introduced through: pkg:maven/org.springframework/spring-web@5.2.14.RELEASE
  URL: https://security.snyk.io/vuln/SNYK-JAVA-ORGSPRINGFRAMEWORK-6444790

× [HIGH] Path Traversal
  Introduced through: pkg:maven/org.springframework/spring-webmvc@5.2.14.RELEASE
  URL: https://security.snyk.io/vuln/SNYK-JAVA-ORGSPRINGFRAMEWORK-7945490

× [HIGH] Path Traversal
  Introduced through: pkg:maven/org.springframework/spring-webmvc@5.2.14.RELEASE
  URL: https://security.snyk.io/vuln/SNYK-JAVA-ORGSPRINGFRAMEWORK-8230373

× [HIGH] Denial of Service (DoS)
  Introduced through: pkg:maven/org.springframework.boot/spring-boot-autoconfigure@2.3.10.RELEASE
  URL: https://security.snyk.io/vuln/SNYK-JAVA-ORGSPRINGFRAMEWORKBOOT-5564390

× [HIGH] Denial of Service (DoS)
  Introduced through: pkg:maven/org.yaml/snakeyaml@1.26
  URL: https://security.snyk.io/vuln/SNYK-JAVA-ORGYAML-2806360

× [HIGH] Denial of Service (DoS)
  Introduced through: pkg:maven/org.yaml/snakeyaml@1.26
  URL: https://security.snyk.io/vuln/SNYK-JAVA-ORGYAML-6056527

× [CRITICAL] Remote Code Execution (RCE)
  Introduced through: pkg:maven/com.h2database/h2@1.4.200
  URL: https://security.snyk.io/vuln/SNYK-JAVA-COMH2DATABASE-2348247

× [CRITICAL] Remote Code Execution (RCE)
  Introduced through: pkg:maven/org.apache.logging.log4j/log4j-core@2.15.0
  URL: https://security.snyk.io/vuln/SNYK-JAVA-ORGAPACHELOGGINGLOG4J-2320014

× [CRITICAL] Uncaught Exception
  Introduced through: pkg:maven/org.apache.tomcat.embed/tomcat-embed-core@9.0.45
  URL: https://security.snyk.io/vuln/SNYK-JAVA-ORGAPACHETOMCATEMBED-8383920

× [CRITICAL] Remote Code Execution
  Introduced through: pkg:maven/org.springframework/spring-beans@5.2.14.RELEASE
  URL: https://security.snyk.io/vuln/SNYK-JAVA-ORGSPRINGFRAMEWORK-2436751

╭──────────────────────────────────────────────────────────────────────╮
│  Test summary                                                        │
│    Organization:    a74651b5-a1c6-4a0a-b4b6-87788fb0f8f6             │
│    Test type:       Software Bill of Materials                       │
│    Path:            apples.json                                      │
│                                                                      │
│    Open issues:     69 [ 4 CRITICAL  26 HIGH  29 MEDIUM  10 LOW ]    │
╰──────────────────────────────────────────────────────────────────────╯
```
- For monorepos we would just use --all-projects as follows. This example is not a monorepo so the output would be identical 

```shell
$ snyk sbom --format=cyclonedx1.4+json --all-projects
```

More command line options and demos can be found here 

https://docs.snyk.io/snyk-cli/commands/sbom

## Snyk SBOM Container demos

The snyk container sbom command generates an SBOM for a container image

- Generate a SBOM for a container image as shown below

```shell
$ snyk container sbom --format=cyclonedx1.4+json pasapples/snyk-boot-web:v1 | jq 
{
  "$schema": "http://cyclonedx.org/schema/bom-1.4.schema.json",
  "bomFormat": "CycloneDX",
  "specVersion": "1.4",
  "version": 1,
  "metadata": {
    "timestamp": "2024-01-02T04:10:19Z",
    "tools": [
      {
        "vendor": "Snyk",
        "name": "SBOM Export API",
        "version": "v1.49.0"
      }
    ],
    "component": {
      "bom-ref": "1-pasapples/snyk-boot-web@v1",
      "type": "container",
      "name": "pasapples/snyk-boot-web",
      "version": "v1"
    }
  },
  "components": [
    {
      "bom-ref": "1-docker-image|pasapples/snyk-boot-web@v1",
      "type": "library",
      "name": "docker-image|pasapples/snyk-boot-web",
      "version": "v1",
      "purl": "pkg:deb/debian/snyk-boot-web@v1?distro=buster&upstream=docker-image%7Cpasapples"
    },
    {
      "bom-ref": "2-acl/libacl1@2.2.53-4",
      "type": "library",
      "name": "acl/libacl1",
      "version": "2.2.53-4",
      "purl": "pkg:deb/debian/libacl1@2.2.53-4?distro=buster&upstream=acl"
    },
    {
      "bom-ref": "3-audit/libaudit-common@1:2.8.4-3",
      "type": "library",
      "name": "audit/libaudit-common",
      "version": "1:2.8.4-3",
      "purl": "pkg:deb/libaudit-common@1:2.8.4-3?upstream=audit"
    },
    
    ....
```

- By default, Snyk scans and generates an SBOM for operating system dependencies as well as application dependencies in your image by default. You can disable generation for application dependencies by adding --exclude-app-vulns as shown below

```shell
$ snyk container sbom --format=cyclonedx1.4+json --exclude-app-vulns pasapples/snyk-boot-web:v1
```

- You can also refer to a container image by it's digest as shown below

```shell
$ snyk container sbom --format=cyclonedx1.4+json alpine@sha256:c5c5fda71656f28e49ac9c5416b3643eaa6a108a8093151d6d1afc9463be8e33 | jq 
{
  "$schema": "http://cyclonedx.org/schema/bom-1.4.schema.json",
  "bomFormat": "CycloneDX",
  "specVersion": "1.4",
  "version": 1,
  "metadata": {
    "timestamp": "2024-01-02T04:15:25Z",
    "tools": [
      {
        "vendor": "Snyk",
        "name": "SBOM Export API",
        "version": "v1.49.0"
      }
    ],
    "component": {
      "bom-ref": "1-alpine@sha256:c5c5fda71656f28e49ac9c5416b3643eaa6a108a8093151d6d1afc9463be8e33",
      "type": "container",
      "name": "alpine",
      "version": "sha256:c5c5fda71656f28e49ac9c5416b3643eaa6a108a8093151d6d1afc9463be8e33"
    }
  },
  "components": [
    {
      "bom-ref": "1-docker-image|alpine@",
      "type": "library",
      "name": "docker-image|alpine",
      "purl": "pkg:apk/alpine/docker-image%7Calpine?distro=alpine-3.18.3"
    },
    {
      "bom-ref": "2-alpine-baselayout/alpine-baselayout-data@3.4.3-r1",
      "type": "library",
      "name": "alpine-baselayout/alpine-baselayout-data",
      "version": "3.4.3-r1",
      "purl": "pkg:apk/alpine/alpine-baselayout-data@3.4.3-r1?distro=alpine-3.18.3&upstream=alpine-baselayout"
    },
    {
      "bom-ref": "3-musl/musl@1.2.4-r1",
      "type": "library",
      "name": "musl/musl",
      "version": "1.2.4-r1",
      "purl": "pkg:apk/alpine/musl@1.2.4-r1?distro=alpine-3.18.3&upstream=musl"
    },
    
    ....
```

More command line options and demos can be found here

https://docs.snyk.io/snyk-cli/commands/container-sbom

## Snyk SBOM API demos

Snyk offers an endpoint to generate SBOM documents for Open Source and Container Projects that are continuously being monitored for issues

On the Snyk Web UI, retrieve your organization ID (UUID format), project ID (UUID) and API key.

* ORG ID - This is the ORG ID inj Snyk that contains the Open Source or container project you wish to generate a SBOM for
* PROJECT ID - This is the Project ID in snyk 
* API key - This is your API key or Service Account in Snyk 

You can use any HTTP client in the examples which follow I will use CURL and [HTTPie](https://httpie.io/)

- In this example we use an open source project to generate an SBOM from 

CURL:

```shell
$ curl --get \
  -H "Authorization: token `snyk config get api`" \
  --data-urlencode "version=2023-03-20" \
  --data-urlencode "format=cyclonedx1.4+json" \
  https://api.snyk.io/rest/orgs/ORG_ID/projects/PROJECT_ID/sbom
{
  "$schema": "http://cyclonedx.org/schema/bom-1.4.schema.json",
  "bomFormat": "CycloneDX",
  "specVersion": "1.4",
  "version": 1,
  "metadata": {
    "timestamp": "2024-01-02T07:08:59Z",
    "tools": [
      {
        "vendor": "Snyk",
        "name": "SBOM Export API",
        "version": "v1.49.0"
      }
    ],
    "component": {
      "bom-ref": "1-com.example:snyk-boot-web@0.0.1-SNAPSHOT",
      "type": "application",
      "name": "com.example:snyk-boot-web",
      "version": "0.0.1-SNAPSHOT",
      "purl": "pkg:maven/com.example/snyk-boot-web@0.0.1-SNAPSHOT"
    },
    "properties": [
      {
        "name": "snyk:org_id",
        "value": "b9d36c95-d54e-4e09-9c62-1c8c3cebd90d"
      },
      {
        "name": "snyk:project_id",
        "value": "1cb68b23-2a81-4f11-bd85-6a9f58555ce5"
      }
    ]
  },
  "components": [
    {
      "bom-ref": "2-com.h2database:h2@1.4.200",
      "type": "library",
      "name": "com.h2database:h2",
      "version": "1.4.200",
      "purl": "pkg:maven/com.h2database/h2@1.4.200"
    },
    
    ....
```

HTTPie:

```shell
$ http "https://api.snyk.io/rest/orgs/ORG-ID/projects/PROJECT-ID/sbom" \
  "Authorization: token `snyk config get api`" \
format=="cyclonedx1.4+json" \
version=="2023-03-20"
HTTP/1.1 200 OK
Akamai-Cache-Status: Miss from child, Miss from parent
Akamai-GRN: 0.1f2ada17.1704180019.655e1f5d
Connection: keep-alive
Content-Length: 20904
Content-Type: application/vnd.cyclonedx+json
Date: Tue, 02 Jan 2024 07:20:21 GMT
Server: envoy
Strict-Transport-Security: max-age=31536000; preload
X-Content-Type-Options: nosniff
X-Frame-Options: SAMEORIGIN
X-Xss-Protection: 1; mode=block
snyk-request-id: 7ee2da33-8251-4544-b463-f01bbc063530
snyk-version-lifecycle-stage: ga
snyk-version-requested: 2023-03-20
snyk-version-served: 2023-03-20

{
    "$schema": "http://cyclonedx.org/schema/bom-1.4.schema.json",
    "bomFormat": "CycloneDX",
    "components": [
        {
            "bom-ref": "2-com.h2database:h2@1.4.200",
            "name": "com.h2database:h2",
            "purl": "pkg:maven/com.h2database/h2@1.4.200",
            "type": "library",
            "version": "1.4.200"
        },
        {
            "bom-ref": "3-org.apache.logging.log4j:log4j-api@2.13.3",
            "name": "org.apache.logging.log4j:log4j-api",
            "purl": "pkg:maven/org.apache.logging.log4j/log4j-api@2.13.3",
            "type": "library",
            "version": "2.13.3"
        },
    
    ....
```

- In this example we use a container project to generate an SBOM from it's an identical command but this time our project ID is a container source project within Snyk so results include container dependencies 

curl:

```shell
$ curl --get \
  -H "Authorization: token `snyk config get api`" \
  --data-urlencode "version=2023-03-20" \
  --data-urlencode "format=cyclonedx1.4+json" \
  https://api.snyk.io/rest/orgs/ORG-ID/projects/PROJECT-ID/sbom
{
  "$schema": "http://cyclonedx.org/schema/bom-1.4.schema.json",
  "bomFormat": "CycloneDX",
  "specVersion": "1.4",
  "version": 1,
  "metadata": {
    "timestamp": "2024-01-02T07:15:24Z",
    "tools": [
      {
        "vendor": "Snyk",
        "name": "SBOM Export API",
        "version": "v1.49.0"
      }
    ],
    "component": {
      "bom-ref": "1-docker-image|pasapples/snyk-boot-web@v1",
      "type": "container",
      "name": "docker-image|pasapples/snyk-boot-web",
      "version": "v1"
    },
    "properties": [
      {
        "name": "snyk:org_id",
        "value": "681d22e1-15b6-44af-8161-8b17c768a16c"
      },
      {
        "name": "snyk:project_id",
        "value": "c3ced45a-aab6-4b65-9d43-9c94418adf7c"
      }
    ]
  },
  "components": [
    {
      "bom-ref": "2-audit/libaudit-common@1:2.8.4-3",
      "type": "library",
      "name": "audit/libaudit-common",
      "version": "1:2.8.4-3",
      "purl": "pkg:deb/debian/libaudit-common@1:2.8.4-3?distro=buster&upstream=audit"
    },
```

HTTPie:

```shell
http "https://api.snyk.io/rest/orgs/ORG-ID/projects/PROJECT-ID/sbom" \
  "Authorization: token `snyk config get api`" \
format=="cyclonedx1.4+json" \
version=="2023-03-20"
HTTP/1.1 200 OK
Akamai-Cache-Status: Miss from child, Miss from parent
Akamai-GRN: 0.1f2ada17.1704179942.655c21c2
Connection: keep-alive, Transfer-Encoding
Content-Type: application/vnd.cyclonedx+json
Date: Tue, 02 Jan 2024 07:19:02 GMT
Server: envoy
Strict-Transport-Security: max-age=31536000; preload
Transfer-Encoding: chunked
X-Content-Type-Options: nosniff
X-Frame-Options: SAMEORIGIN
X-Xss-Protection: 1; mode=block
snyk-request-id: 3290a3dd-0832-47bb-b425-e8f977c800d2
snyk-version-lifecycle-stage: ga
snyk-version-requested: 2023-03-20
snyk-version-served: 2023-03-20

{
    "$schema": "http://cyclonedx.org/schema/bom-1.4.schema.json",
    "bomFormat": "CycloneDX",
    "components": [
        {
            "bom-ref": "2-audit/libaudit-common@1:2.8.4-3",
            "name": "audit/libaudit-common",
            "purl": "pkg:deb/debian/libaudit-common@1:2.8.4-3?distro=buster&upstream=audit",
            "type": "library",
            "version": "1:2.8.4-3"
        },
        {
            "bom-ref": "3-libcap-ng/libcap-ng0@0.7.9-2",
            "name": "libcap-ng/libcap-ng0",
            "purl": "pkg:deb/debian/libcap-ng0@0.7.9-2?distro=buster&upstream=libcap-ng",
            "type": "library",
            "version": "0.7.9-2"
        },
        
        ....
```

More options and information can be found here 

https://docs.snyk.io/snyk-api/get-a-projects-sbom-document-endpoint

<hr />
Pas Apicella [pas at snyk.io] is a Principal Solution Engineer at Snyk APJ 