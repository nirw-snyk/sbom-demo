# SBOM Testing Options

## Test SBOM web page:
Web based  [Snyk SBOM Security Checker](https://snyk.io/code-checker/sbom-security/)

## Test SBOM using snyk CLI- see full help:
```
➜  easybuggy git:(master) ✗ snyk sbom test --help
SBOM test
  Feature availability: This feature is available to customers on Snyk Enterprise plans.

Usage
  snyk sbom test --experimental --file=<FILE_PATH> [<options>]

Description
  The snyk sbom test command checks SBOM files for vulnerabilities in open-source packages.

Exit codes
  Possible exit codes and their meaning:

  0: success (scan completed), no vulnerabilities found
  1: action_needed (scan completed), vulnerabilities found
  2: failure, try to re-run the command

Configure the Snyk CLI
  You can use environment variables to configure the Snyk CLI and set variables for connecting with
  the Snyk API. See Configure the Snyk CLI https://docs.snyk.io/snyk-cli/configure-the-snyk-cli

Debug
  Use the -d or --debug option to output the debug logs.

Options
  --experimental
    Required. Use experimental command features. This option is currently required as the command is
    in its experimental phase.

  --file=<FILE_PATH>
    Required. Specify the file path of the SBOM document.

    The snyk sbom test command accepts the following file formats:

    -  CycloneDX: JSON version 1.4 and 1.5
    -  SPDX: JSON version 2.3

    Packages and components within the provided SBOM file must be identified by a PackageURL (purl).

    Supported purl types are: apk, cargo, cocoapods, composer, deb, gem, generic, golang, hex, maven
    , npm, nuget, pub, pypi, rpm, swift.

    Example: $ snyk sbom test --experimental --file=bom.cdx.json

  --json
    Print results on the console as a JSON data structure.

    Example: $ snyk sbom test --experimental --file=bom.cdx.json --json
```
##### Test a cyclonedx1.4.json sbom file using snyk sbom test cli:
```
➜  easybuggy git:(master) ✗ snyk sbom test --experimental --file=mysbom.cyc1.4.json

Testing mysbom.cyc1.4.json


Open issues:

× [LOW] Information Exposure
  Introduced through: pkg:maven/commons-codec/commons-codec@1.2
  URL: https://security.snyk.io/vuln/SNYK-JAVA-COMMONSCODEC-561518

× [LOW] Man-in-the-Middle (MitM)
  Introduced through: pkg:maven/log4j/log4j@1.2.13
  URL: https://security.snyk.io/vuln/SNYK-JAVA-LOG4J-1300176

× [LOW] Improper Access Control
  Introduced through: pkg:maven/mysql/mysql-connector-java@5.1.25
  URL: https://security.snyk.io/vuln/SNYK-JAVA-MYSQL-31449
....

× [HIGH] Denial of Service (DoS)
  Introduced through: pkg:maven/xerces/xercesImpl@2.8.0
  URL: https://security.snyk.io/vuln/SNYK-JAVA-XERCES-2359991

× [HIGH] Denial of Service (DoS)
  Introduced through: pkg:maven/xerces/xercesImpl@2.8.0
  URL: https://security.snyk.io/vuln/SNYK-JAVA-XERCES-31585

× [HIGH] GPL-2.0 license
  Introduced through: pkg:maven/mysql/mysql-connector-java@5.1.25
  URL: https://security.snyk.io/vuln/snyk:lic:maven:mysql:mysql-connector-java:GPL-2.0

× [CRITICAL] Arbitrary Code Execution
  Introduced through: pkg:maven/commons-fileupload/commons-fileupload@1.3.1
  URL: https://security.snyk.io/vuln/SNYK-JAVA-COMMONSFILEUPLOAD-30401

× [CRITICAL] Deserialization of Untrusted Data
  Introduced through: pkg:maven/log4j/log4j@1.2.13
  URL: https://security.snyk.io/vuln/SNYK-JAVA-LOG4J-572732

× [CRITICAL] Remote Code Execution (RCE)
  Introduced through: pkg:maven/org.apache.logging.log4j/log4j-core@2.1
  URL: https://security.snyk.io/vuln/SNYK-JAVA-ORGAPACHELOGGINGLOG4J-2314720

× [CRITICAL] Remote Code Execution (RCE)
  Introduced through: pkg:maven/org.apache.logging.log4j/log4j-core@2.1
  URL: https://security.snyk.io/vuln/SNYK-JAVA-ORGAPACHELOGGINGLOG4J-2320014

× [CRITICAL] Deserialization of Untrusted Data
  Introduced through: pkg:maven/org.apache.logging.log4j/log4j-core@2.1
  URL: https://security.snyk.io/vuln/SNYK-JAVA-ORGAPACHELOGGINGLOG4J-31409

× [CRITICAL] Arbitrary Code Execution
  Introduced through: pkg:maven/xalan/xalan@2.7.0
  URL: https://security.snyk.io/vuln/SNYK-JAVA-XALAN-2953385

╭─────────────────────────────────────────────────────────────────────╮
│  Test summary                                                       │
│    Organization:    2fe09a63-6bf6-4d85-8cba-1064663760ff            │
│    Test type:       Software Bill of Materials                      │
│    Path:            mysbom.cyc1.4.json                              │
│                                                                     │
│    Open issues:     62 [ 6 CRITICAL  16 HIGH  35 MEDIUM  5 LOW ]    │
╰─────────────────────────────────────────────────────────────────────╯
```
example file [here](https://github.com/nirw-snyk/sbom-demo/blob/main/samples/myCyclonedx1.4.json)

>Note: the raw json output was beautified

## Test SBOM using API:

#### Step 1: Post an SBOM test request
https://apidocs.snyk.io/experimental?version=2024-02-21%7Eexperimental#get-/orgs/-org_id-/projects/-project_id-/sbom
>POST /orgs/{org_id}/sbom_tests

using curl:
```
curl -X POST "<Snyk-REST-Base-URL>/orgs/<Org-Id>/sbom_tests?version=<API-Version>" \
 -H "accept: application/vnd.api+json"\
 -H "authorization: <API-TOKEN>"\
 -H "content-type: application/vnd.api+json" \
 -d '{"data":{"type":"sbom_test","attributes":{"sbom":<paste-your-sbom-content-here>}}}' \
```
Successful response (201) would give a job-id which could then be used for the next API call.
In the example below the job-id is 
>34e8117b-7417-42d5-af17-dd588e52843a
```
{
  "data": {
    "id": "34e8117b-7417-42d5-af17-dd588e52843a",
    "type": "sbom_tests"
  },
  "jsonapi": {
    "version": "1.0"
  },
  "links": {
    "self": "/rest/orgs/<Org-Id>/sbom_tests?version=<API-Version>",
    "related": "/rest/orgs/<Org-Id>/sbom_tests/34e8117b-7417-42d5-af17-dd588e52843a?version=<API-Version>"
  }
}
```
#### Step 2: Get SBOM Test results from Job Id:
https://apidocs.snyk.io/experimental?version=2024-02-21%7Eexperimental#get-/orgs/-org_id-/sbom_tests/-job_id-/results
>GET /orgs/{org_id}/sbom_tests/{job_id}/results

using curl:
```
curl -X GET "<Snyk-REST-Base-URL>/orgs/<Org-Id>/sbom_tests/<Job-Id>/results?version=<API-Version>" \
 -H "accept: application/vnd.api+json"\
 -H "authorization: <API-TOKEN>"\
```

Postman collection [here](https://github.com/nirw-snyk/sbom-demo/blob/main/SBOM.postman_collection.json)

example file [here](https://github.com/nirw-snyk/sbom-demo/blob/main/samples/myCyclonedx1.4-Tested.json)
                    
## Test SBOM using CLI and a 3rd party - bomber:
CLI using 3rd party tool [bomber and snyk sbom provider](https://github.com/devops-kung-fu/bomber)

### Step 1: Install bomber (mac)
```
brew tap devops-kung-fu/homebrew-tap
brew install devops-kung-fu/homebrew-tap/bomber
```

### Step 2: bomber --help
```
 easybuggy git:(demo-feature-branch) ✗ bomber --help
Scans SBOMs for security vulnerabilities.

Usage:
  bomber [command]

Examples:
  bomber scan --output html test.cyclonedx.json

Available Commands:
  completion  Generate the autocompletion script for the specified shell
  help        Help about any command
  scan        Scans a provided SBOM file or folder containing SBOMs for vulnerabilities.

Flags:
      --debug           displays debug level log messages.
  -h, --help            help for bomber
      --output string   how bomber should output findings (json, html, stdout) (default "stdout")
  -v, --version         version for bomber

Use "bomber [command] --help" for more information about a command.
```


### Step 3: test SBOM using bomber and snyk provider (note currently only MT-US) snyk CLI is planned to support this natively during H1 2024
bomber using stdout (default):
```
 easybuggy git:(demo-feature-branch) ✗ bomber scan --provider snyk --token $SNYK_MT_US_TOKEN myCyclonedx1.4.json

 ██▄ ▄▀▄ █▄ ▄█ ██▄ ██▀ █▀▄
 █▄█ ▀▄▀ █ ▀ █ █▄█ █▄▄ █▀▄

DKFM - DevOps Kung Fu Mafia
https://github.com/devops-kung-fu/bomber
Version: 0.4.8

■ Ecosystems detected: maven
■ Scanning 37 packages for vulnerabilities...
■ Vulnerability Provider: Snyk (https://security.snyk.io)

■ Files Scanned
	myCyclonedx1.4.json (sha256:3910662a2db8a4edc37c6b91bfd71c3e795a54782ae030a6f2160400ad4f26ac)

╭───────┬──────────────────────┬──────────┬──────────┬──────────────────────────────────────────┬────────╮
│ TYPE  │ NAME                 │ VERSION  │ SEVERITY │ VULNERABILITY                            │ EPSS % │
├───────┼──────────────────────┼──────────┼──────────┼──────────────────────────────────────────┼────────┤
│ maven │ xalan                │ 2.7.0    │ HIGH     │ SNYK-JAVA-XALAN-31385                    │ 77%    │
│       │                      ├──────────┼──────────┼──────────────────────────────────────────┼────────┤
│       │                      │ 2.7.0    │ CRITICAL │ SNYK-JAVA-XALAN-2953385                  │ 52%    │
│       ├──────────────────────┼──────────┼──────────┼──────────────────────────────────────────┼────────┤
│       │ nekohtml             │ 1.9.16   │ MODERATE │ SNYK-JAVA-NETSOURCEFORGENEKOHTML-2774754 │ 43%    │
│       │                      ├──────────┼──────────┼──────────────────────────────────────────┼────────┤
│       │                      │ 1.9.16   │ MODERATE │ SNYK-JAVA-NETSOURCEFORGENEKOHTML-2803036 │ 35%    │
│       │                      ├──────────┼──────────┼──────────────────────────────────────────┼────────┤
│       │                      │ 1.9.16   │ HIGH     │ SNYK-JAVA-NETSOURCEFORGENEKOHTML-2621454 │ 56%    │
│       ├──────────────────────┼──────────┼──────────┼──────────────────────────────────────────┼────────┤
│       │ mysql-connector-java │ 5.1.25   │ MODERATE │ SNYK-JAVA-MYSQL-451460                   │ 55%    │
│       │                      ├──────────┼──────────┼──────────────────────────────────────────┼────────┤
│       │                      │ 5.1.25   │ MODERATE │ SNYK-JAVA-MYSQL-31580                    │ 45%    │
│       │                      ├──────────┼──────────┼──────────────────────────────────────────┼────────┤
│       │                      │ 5.1.25   │ MODERATE │ SNYK-JAVA-MYSQL-174574                   │ 13%    │
│       │                      ├──────────┼──────────┼──────────────────────────────────────────┼────────┤
│       │                      │ 5.1.25   │ MODERATE │ SNYK-JAVA-MYSQL-2386864                  │ 39%    │
│       │                      ├──────────┼──────────┼──────────────────────────────────────────┼────────┤
│       │                      │ 5.1.25   │ MODERATE │ SNYK-JAVA-MYSQL-1766958                  │ 25%    │
│       │                      ├──────────┼──────────┼──────────────────────────────────────────┼────────┤
│       │                      │ 5.1.25   │ LOW      │ SNYK-JAVA-MYSQL-31449                    │ 18%    │
│       │                      ├──────────┼──────────┼──────────────────────────────────────────┼────────┤
│       │                      │ 5.1.25   │ HIGH     │ SNYK-JAVA-MYSQL-31399                    │ 50%    │
│       │                      ├──────────┼──────────┼──────────────────────────────────────────┼────────┤
│       │                      │ 5.1.25   │ HIGH     │ SNYK-JAVA-MYSQL-451464                   │ 68%    │
│       ├──────────────────────┼──────────┼──────────┼──────────────────────────────────────────┼────────┤
│       │ log4j-core           │ 2.1      │ MODERATE │ SNYK-JAVA-ORGAPACHELOGGINGLOG4J-2327339  │ 90%    │
│       │                      ├──────────┼──────────┼──────────────────────────────────────────┼────────┤
│       │                      │ 2.1      │ LOW      │ SNYK-JAVA-ORGAPACHELOGGINGLOG4J-567761   │ 54%    │
│       │                      ├──────────┼──────────┼──────────────────────────────────────────┼────────┤
│       │                      │ 2.1      │ HIGH     │ SNYK-JAVA-ORGAPACHELOGGINGLOG4J-2321524  │ 100%   │
│       │                      ├──────────┼──────────┼──────────────────────────────────────────┼────────┤
│       │                      │ 2.1      │ CRITICAL │ SNYK-JAVA-ORGAPACHELOGGINGLOG4J-31409    │ 98%    │
│       │                      ├──────────┼──────────┼──────────────────────────────────────────┼────────┤
│       │                      │ 2.1      │ CRITICAL │ SNYK-JAVA-ORGAPACHELOGGINGLOG4J-2320014  │ 100%   │
│       │                      ├──────────┼──────────┼──────────────────────────────────────────┼────────┤
│       │                      │ 2.1      │ CRITICAL │ SNYK-JAVA-ORGAPACHELOGGINGLOG4J-2314720  │ 100%   │
│       ├──────────────────────┼──────────┼──────────┼──────────────────────────────────────────┼────────┤
│       │ log4j                │ 1.2.13   │ MODERATE │ SNYK-JAVA-LOG4J-3358774                  │ 46%    │
│       │                      ├──────────┼──────────┼──────────────────────────────────────────┼────────┤
│       │                      │ 1.2.13   │ MODERATE │ SNYK-JAVA-LOG4J-2316893                  │ 95%    │
│       │                      ├──────────┼──────────┼──────────────────────────────────────────┼────────┤
│       │                      │ 1.2.13   │ LOW      │ SNYK-JAVA-LOG4J-1300176                  │ 54%    │
│       │                      ├──────────┼──────────┼──────────────────────────────────────────┼────────┤
│       │                      │ 1.2.13   │ HIGH     │ SNYK-JAVA-LOG4J-2342646                  │ 83%    │
│       │                      ├──────────┼──────────┼──────────────────────────────────────────┼────────┤
│       │                      │ 1.2.13   │ HIGH     │ SNYK-JAVA-LOG4J-2342645                  │ 76%    │
│       │                      ├──────────┼──────────┼──────────────────────────────────────────┼────────┤
│       │                      │ 1.2.13   │ HIGH     │ SNYK-JAVA-LOG4J-2342647                  │ 78%    │
│       │                      ├──────────┼──────────┼──────────────────────────────────────────┼────────┤
│       │                      │ 1.2.13   │ CRITICAL │ SNYK-JAVA-LOG4J-572732                   │ 98%    │
│       ├──────────────────────┼──────────┼──────────┼──────────────────────────────────────────┼────────┤
│       │ jstl                 │ 1.2      │ HIGH     │ SNYK-JAVA-JAVAXSERVLET-30449             │ 93%    │
│       ├──────────────────────┼──────────┼──────────┼──────────────────────────────────────────┼────────┤
│       │ esapi                │ 2.1.0.1  │ MODERATE │ SNYK-JAVA-ORGOWASPESAPI-2805301          │ 55%    │
│       │                      ├──────────┼──────────┼──────────────────────────────────────────┼────────┤
│       │                      │ 2.1.0.1  │ LOW      │ SNYK-JAVA-ORGOWASPESAPI-1088594          │ N/A    │
│       │                      ├──────────┼──────────┼──────────────────────────────────────────┼────────┤
│       │                      │ 2.1.0.1  │ HIGH     │ SNYK-JAVA-ORGOWASPESAPI-6038553          │ N/A    │
│       │                      ├──────────┼──────────┼──────────────────────────────────────────┼────────┤
│       │                      │ 2.1.0.1  │ HIGH     │ SNYK-JAVA-ORGOWASPESAPI-6091110          │ N/A    │
│       │                      ├──────────┼──────────┼──────────────────────────────────────────┼────────┤
│       │                      │ 2.1.0.1  │ HIGH     │ SNYK-JAVA-ORGOWASPESAPI-2803305          │ 69%    │
│       ├──────────────────────┼──────────┼──────────┼──────────────────────────────────────────┼────────┤
│       │ derby                │ 10.8.3.0 │ MODERATE │ SNYK-JAVA-ORGAPACHEDERBY-6069877         │ 61%    │
│       │                      ├──────────┼──────────┼──────────────────────────────────────────┼────────┤
│       │                      │ 10.8.3.0 │ MODERATE │ SNYK-JAVA-ORGAPACHEDERBY-32274           │ 48%    │
│       ├──────────────────────┼──────────┼──────────┼──────────────────────────────────────────┼────────┤
│       │ commons-io           │ 2.2      │ MODERATE │ SNYK-JAVA-COMMONSIO-1277109              │ 54%    │
│       ├──────────────────────┼──────────┼──────────┼──────────────────────────────────────────┼────────┤
│       │ commons-httpclient   │ 3.1      │ MODERATE │ SNYK-JAVA-COMMONSHTTPCLIENT-30083        │ 61%    │
│       │                      ├──────────┼──────────┼──────────────────────────────────────────┼────────┤
│       │                      │ 3.1      │ MODERATE │ SNYK-JAVA-COMMONSHTTPCLIENT-31660        │ 51%    │
│       ├──────────────────────┼──────────┼──────────┼──────────────────────────────────────────┼────────┤
│       │ commons-fileupload   │ 1.3.1    │ MODERATE │ SNYK-JAVA-COMMONSFILEUPLOAD-3326457      │ 92%    │
│       │                      ├──────────┼──────────┼──────────────────────────────────────────┼────────┤
│       │                      │ 1.3.1    │ MODERATE │ SNYK-JAVA-COMMONSFILEUPLOAD-31540        │ N/A    │
│       │                      ├──────────┼──────────┼──────────────────────────────────────────┼────────┤
│       │                      │ 1.3.1    │ HIGH     │ SNYK-JAVA-COMMONSFILEUPLOAD-30082        │ 92%    │
│       │                      ├──────────┼──────────┼──────────────────────────────────────────┼────────┤
│       │                      │ 1.3.1    │ CRITICAL │ SNYK-JAVA-COMMONSFILEUPLOAD-30401        │ 93%    │
│       ├──────────────────────┼──────────┼──────────┼──────────────────────────────────────────┼────────┤
│       │ commons-codec        │ 1.2      │ LOW      │ SNYK-JAVA-COMMONSCODEC-561518            │ N/A    │
│       ├──────────────────────┼──────────┼──────────┼──────────────────────────────────────────┼────────┤
│       │ antisamy             │ 1.5.3    │ MODERATE │ SNYK-JAVA-ORGOWASPANTISAMY-2774682       │ 49%    │
│       │                      ├──────────┼──────────┼──────────────────────────────────────────┼────────┤
│       │                      │ 1.5.3    │ MODERATE │ SNYK-JAVA-ORGOWASPANTISAMY-6227504       │ 11%    │
│       │                      ├──────────┼──────────┼──────────────────────────────────────────┼────────┤
│       │                      │ 1.5.3    │ MODERATE │ SNYK-JAVA-ORGOWASPANTISAMY-5950399       │ 11%    │
│       │                      ├──────────┼──────────┼──────────────────────────────────────────┼────────┤
│       │                      │ 1.5.3    │ MODERATE │ SNYK-JAVA-ORGOWASPANTISAMY-31591         │ 69%    │
│       │                      ├──────────┼──────────┼──────────────────────────────────────────┼────────┤
│       │                      │ 1.5.3    │ MODERATE │ SNYK-JAVA-ORGOWASPANTISAMY-598767        │ 49%    │
│       │                      ├──────────┼──────────┼──────────────────────────────────────────┼────────┤
│       │                      │ 1.5.3    │ MODERATE │ SNYK-JAVA-ORGOWASPANTISAMY-1320080       │ 37%    │
│       │                      ├──────────┼──────────┼──────────────────────────────────────────┼────────┤
│       │                      │ 1.5.3    │ MODERATE │ SNYK-JAVA-ORGOWASPANTISAMY-2774681       │ 24%    │
╰───────┴──────────────────────┴──────────┴──────────┴──────────────────────────────────────────┴────────╯

Total vulnerabilities found: 49

╭──────────┬───────╮
│ RATING   │ COUNT │
├──────────┼───────┤
│ CRITICAL │     6 │
├──────────┼───────┤
│ HIGH     │    13 │
├──────────┼───────┤
│ MODERATE │    25 │
├──────────┼───────┤
│ LOW      │     5 │
╰──────────┴───────╯


NOTES:

1. The list of vulnerabilities displayed may differ from provider to provider. This list
   may not contain all possible vulnerabilities. Please try the other providers that bomber
   supports (osv, ossindex, snyk)
2. EPSS Percentage indicates the % chance that the vulnerability will be exploited. This
   value will assist in prioritizing remediation. For more information on EPSS, refer to
   https://www.first.org/epss/
➜  easybuggy git:(demo-feature-branch) ✗
```

```
 easybuggy git:(demo-feature-branch) ✗ bomber scan --provider snyk --token $SNYK_MT_US_TOKEN --output html myCyclonedx1.4.json

 ██▄ ▄▀▄ █▄ ▄█ ██▄ ██▀ █▀▄
 █▄█ ▀▄▀ █ ▀ █ █▄█ █▄▄ █▀▄

DKFM - DevOps Kung Fu Mafia
https://github.com/devops-kung-fu/bomber
Version: 0.4.8

■ Ecosystems detected: maven
■ Scanning 37 packages for vulnerabilities...
■ Vulnerability Provider: Snyk (https://security.snyk.io)

■ Writing filename: 20240314-22-39-03-bomber-results.html
➜  easybuggy git:(demo-feature-branch) ✗

```
bomber html report sample [here](https://htmlpreview.github.io/?https://github.com/nirw-snyk/sbom-demo/blob/main/samples/20240314-22-39-03-bomber-results.html)


###
Next: [SBOM Enriching Options](https://github.com/nirw-snyk/sbom-demo/blob/main/SBOM-Enriching-Options.md)