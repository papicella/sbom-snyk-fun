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

- You can also refer to a contaimner image by it's digest as shown below

```shell
$ snyk container sbom --format=cyclonedx1.4+json alpine@sha256:c5c5fda71656f28e49ac9c5416b3643eaa6a108a8093151d6d1afc9463be8e33 | jq | more
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

## Snyk SBOM API demos

<hr />
Pas Apicella [pas at snyk.io] is a Principal Solution Engineer at Snyk APJ 