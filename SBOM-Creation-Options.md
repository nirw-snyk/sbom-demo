# SBOM file Creation Options
#### The first way we will generate an SBOM would be using the snyk UI on a target level (FF must be enabled by your account team):

![sbom.png preview](https://github.com/nirw-snyk/sbom-demo/blob/main/images/sbom.png)

#### Next we can generate an SBOM using snyk CLI - see full help:
```
➜  easybuggy git:(demo-feature-branch) ✗ snyk sbom --help
SBOM
Prerequisites
  Feature availability: This feature is available to customers on Snyk Enterprise plans.

  Note: In order to run the SBOM generation feature, you must use a minimum of CLI version 1.1071.0.

  The snyk sbom feature requires an internet connection.

Usage
  $ snyk sbom --format=<cyclonedx1.4+json|cyclonedx1.4+xml|cyclonedx1.5+json|cyclonedx1.5+xml|spdx
  2.3+json> [--org=<ORG_ID>] [--file=<FILE>] [--unmanaged] [--dev] [--all-projects]
  [--name=<NAME>] [--version=<VERSION>] [--exclude=<NAME>[,<NAME>...]]
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

>Note: the raw json output was beautified


##### create a cyclonedx1.4.xml sbom file using snyk sbom cli:
 snyk sbom --format=cyclonedx1.4+xml --all-projects >&myCyclonedx1.4.xml
example file [here](https://github.com/nirw-snyk/sbom-demo/blob/main/samples/myCyclonedx1.4.xml)

>Note: the raw XML output was beautified


#### create a spdx2.3.json file using snyk Get project's SBOM ducument API:
https://apidocs.snyk.io/experimental?version=2024-02-21%7Eexperimental#get-/orgs/-org_id-/projects/-project_id-/sbom
>GET /orgs/{org_id}/projects/{project_id}/sbom

using curl:
```
curl -X GET "<Snyk-REST-Base-URL>/orgs/<Org-Id>/projects/<Project-Id>/sbom?version=<API-Version>&format=spdx2.3%2Bjson" \
 -H "accept: application/json"\
 -H "authorization: Token <API-Token>" \
```
using the Postman collection [here](https://github.com/nirw-snyk/sbom-demo/blob/main/SBOM.postman_collection.json)

example file [here](https://github.com/nirw-snyk/sbom-demo/blob/main/samples/mySpdx2.3.json)


#### create a cyclondx1.4.json file using snyk Get a target’s SBOM document API:
https://apidocs.snyk.io/experimental?version=2024-02-21%7Eexperimental#get-/orgs/-org_id-/targets/-target_id-/sbom
>GET /orgs/{org_id}/targets/{target_id}/sbom

using curl:
```
curl -X GET "<Snyk-REST-Base-URL>/orgs/<Org-Id>/targets/<Target-Id>/sbom?version=<API-Version>&format=cyclonedx1.4%2Bjson" \
 -H "accept: application/json"\
 -H "authorization: Token <API-Token>" \
```
using the Postman collection [here](https://github.com/nirw-snyk/sbom-demo/blob/main/SBOM.postman_collection.json)

example file [here](https://github.com/nirw-snyk/sbom-demo/blob/main/samples/myTargetCyclondx1.4.json)

#### create a cyclondx1.4.json file using snyk Get a collection’s SBOM document API:
https://apidocs.snyk.io/experimental?version=2024-02-21%7Eexperimental#get-/orgs/-org_id-/collections/-collection_id-/sbom
>GET /orgs/{org_id}/collections/{collection_id}/sbom

using curl:
```
curl -X GET "<Snyk-REST-Base-URL>/orgs/<Org-Id>/collections/<Collection-Id>/sbom?version=<API-Version>&format=cyclonedx1.4%2Bjson" \
 -H "accept: application/json"\
 -H "authorization: Token <API-Token>" \
```
using the Postman collection [here](https://github.com/nirw-snyk/sbom-demo/blob/main/SBOM.postman_collection.json)

example file [here](https://github.com/nirw-snyk/sbom-demo/blob/main/samples/myCollectionCyclondx1.4.json)






###
Next: [SBOM Testing Options](https://github.com/nirw-snyk/sbom-demo/blob/main/SBOM-Testing-Options.md)