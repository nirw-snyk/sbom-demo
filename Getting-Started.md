# Getting Started before running the SBOM Demo

### Running some tests to warm up and check the environment is working
configuring non-default endpoint + snyk token:
```
export SNYK_TOKEN=<my-token>
export SNYK_ORG_ID=<my-org-id>
snyk config set endpoint=https://app.au.snyk.io/api
➜  easybuggy git:(demo-feature-branch) ✗ snyk config set endpoint=https://app.au.snyk.io/api
endpoint updated
snyk auth $SNYK_TOKEN

Your account has been authenticated. Snyk is now ready to be used.

```

run snyk code test (SAST) as sanity check:
```
easybuggy git:(demo-feature-branch) ✗ snyk code test --org=$SNYK_ORG_ID

Testing /Users/nir.weinberg/Documents/src/easybuggy ...
....
 ✗ [High] Server-Side Request Forgery (SSRF)
   Path: src/main/java/org/t246osslab/easybuggy/troubles/NetworkSocketLeakServlet.java, line 35
   Info: Unsanitized input from an HTTP parameter flows into openConnection, where it is used as an URL to perform a request. This may result in a Server-Side Request Forgery vulnerability.


✔ Test completed

Organization:      25812edc-8b7b-4f30-b0fd-3de0421b54aa
Test type:         Static code analysis
Project path:      /Users/nir.weinberg/Documents/src/easybuggy

Summary:

  25 Code issues found
  14 [High]   6 [Medium]   5 [Low]

```

run snyk test (SCA) as sanity check:
```
 easybuggy git:(demo-feature-branch) ✗ snyk test -all-projects  --org=$SNYK_ORG_ID
 Testing /Users/nir.weinberg/Documents/src/easybuggy...

Tested 37 dependencies for known issues, found 61 issues, 61 vulnerable paths.
...

Organization:      REDUCTEC
Package manager:   maven
Target file:       pom.xml
Project name:      org.t246osslab.easybuggy:easybuggy
Open source:       no
Project path:      /Users/nir.weinberg/Documents/src/easybuggy
Licenses:          enabled

⚠ WARNING: Critical severity vulnerabilities were found with Log4j!
  - SNYK-JAVA-ORGAPACHELOGGINGLOG4J-2314720 (See https://security.snyk.io/vuln/SNYK-JAVA-ORGAPACHELOGGINGLOG4J-2314720)

We highly recommend fixing this vulnerability. If it cannot be fixed by upgrading, see mitigation information here:
  - https://security.snyk.io/vuln/SNYK-JAVA-ORGAPACHELOGGINGLOG4J-2314720
  - https://snyk.io/blog/log4shell-remediation-cheat-sheet/
```

run the same tests using [snyk-to-html](https://docs.snyk.io/snyk-cli/scan-and-maintain-projects-using-the-cli/cli-tools/snyk-to-html) for generating html reports:
```
easybuggy git:(demo-feature-branch) ✗ snyk code test --org=$SNYK_ORG_ID --json | snyk-to-html -o SAST-report.html
Vulnerability snapshot saved at SAST-report.html
➜  easybuggy git:(demo-feature-branch) ✗ snyk test -all-projects  --org=$SNYK_ORG_ID --json | snyk-to-html -o SCA-report.html
Vulnerability snapshot saved at SCA-report.html
➜  easybuggy git:(demo-feature-branch) ✗ ls -ltr *.html
-rw-r--r--  1 nir.weinberg  staff  419949 12 Mar 12:35 SAST-report.html
-rw-r--r--  1 nir.weinberg  staff  317539 12 Mar 12:35 SCA-report.html
```
## Reports example:
### SAST:
![sast.png preview](https://github.com/nirw-snyk/sbom-demo/blob/main/images/sast.png)

### SCA:
![sca.png preview](https://github.com/nirw-snyk/sbom-demo/blob/main/images/sca.png)

Next: [SBOM Testing Options](https://github.com/nirw-snyk/sbom-demo/blob/main/SBOM-Creation-Options.md)