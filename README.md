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

