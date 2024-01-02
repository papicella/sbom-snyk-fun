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

## Snyk SBOM API demos

<hr />
Pas Apicella [pas at snyk.io] is a Principal Solution Engineer at Snyk APJ 