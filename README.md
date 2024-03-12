# SBOM Demo Lab
This lab will demonstrate generating / testing and enreaching sbom files using snyk.

## Pre-requisites:
- Having a snyk account/token/cli
- Having a sample code to run SCA tests on which will be used for the sbom. In this lab the [following](https://github.com/nirw-snyk/easybuggy) repo is used for the demo purpose
- other 3rd party tools
    - To view CLI resulys in a friendly html format, we'll use [snyk-to-html](https://docs.snyk.io/snyk-cli/scan-and-maintain-projects-using-the-cli/cli-tools/snyk-to-html)
    - To test snyk generated sbom for vulns using the CLI, we'll use the snyk provider for [bomber](https://github.com/devops-kung-fu/bomber)


### Running some tests to warm up and check the environment is working
Please make sure you followed the steps in [Getting-Started](https://github.com/nirw-snyk/sbom-demo/blob/main/Getting-Started.md) before running the steps described below.

#### snyk sbom full help
```
➜  easybuggy git:(demo-feature-branch) ✗ snyk sbom --help
SBOM
Prerequisites
  Feature availability: This feature is available to customers on Snyk Enterprise plans.

  Note: In order to run the SBOM generation feature, you must use a minimum of CLI version 1.1071.0.

  The snyk sbom feature requires an internet connection.

Usage
  $ snyk sbom --format=<cyclonedx1.4+json|cyclonedx1.4+xml|spdx2.3+json> [--file=<FILE>]
  [--unmanaged] [--org=<ORG_ID>] [--dev] [--all-projects] [--name=<NAME>]
  [--version=<VERSION>] [--exclude=<NAME>[,<NAME>...]]
  [--detection-depth=<DEPTH>] [--prune-repeated-subdependencies|-p] [--maven-aggregate-project]
  [--scan-unmanaged] [--scan-all-unmanaged] [--sub-project=<NAME>]
  [--gradle-sub-project=<NAME>] [--all-sub-projects]
  [--configuration-matching=<CONFIGURATION_REGEX>]
  [--configuration-attributes=<ATTRIBUTE>[,<ATTRIBUTE>]] [--init-script=<FILE>]
  [--json-file-output=<OUTPUT_FILE_PATH>] [<TARGET_DIRECTORY>]

Description
  The snyk sbom command generates an SBOM for a local software project in an ecosystem supported by
  Snyk.
  ...
Create a CycloneDX JSON document for a monorepo
$ snyk sbom --format=cyclonedx1.4+json --all-projects
  ```
##### create a cyclonedx1.4.json sbom file using snyk sbom cli:
```
easybuggy git:(demo-feature-branch) ✗ snyk sbom --format=cyclonedx1.4+json --all-projects --json-file-output=myCyclonedx1.4.json
```
example file [here](https://github.com/nirw-snyk/sbom-demo/blob/main/samples/myCyclonedx1.4.json)

note: the json raw json output was beautified


##### create a cyclonedx1.4.xml sbom file using snyk sbom cli:
 snyk sbom --format=cyclonedx1.4+xml --all-projects >&myCyclonedx1.4.xml
example file [here](https://github.com/nirw-snyk/sbom-demo/blob/main/samples/myCyclonedx1.4.xml)

#### create an spdx2.3.json file using snyk Get project's SBOM ducument API:
>GET /orgs/{org_id}/projects/{project_id}/sbom

using curl:
```
curl -X GET "<Snyk-REST-Base-URL>/orgs/<Org-Id>/projects/<Project-Id>/sbom?version=<API-Version>&format=cyclonedx1.4%2Bjson" \
 -H "accept: application/json"\
 -H "authorization: Token <API-Token>" \
```
using the Postman collection [here](https://github.com/nirw-snyk/sbom-demo/blob/main/SBOM.postman_collection.json)
example file [here](https://github.com/nirw-snyk/sbom-demo/blob/main/samples/mySpdx2.3.json)
