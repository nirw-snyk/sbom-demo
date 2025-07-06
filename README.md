# SBOM Demo Lab
This lab will demonstrate generating / testing and enriching sbom files using snyk.

## Pre-requisites:
- Having a snyk account/token/cli
- Having a sample code to run SCA tests on which will be used for the sbom. In this lab the [following](https://github.com/nirw-snyk/easybuggy) repo is used for the demo purpose
- other 3rd party tools
    - To view CLI results in a friendly html format, we'll use [snyk-to-html](https://docs.snyk.io/snyk-cli/scan-and-maintain-projects-using-the-cli/cli-tools/snyk-to-html)
    - To enrich an SBOM, we'll use [parlay](https://github.com/snyk/parlay)


### Running some tests to warm up and check the environment is working
Make sure you followed the steps in [Getting Started](https://github.com/nirw-snyk/sbom-demo/blob/main/Getting-Started.md) before running the steps described below.

### Part 1: Creating SBOMs options
To start creating SBOM files follow the steps described in [SBOM Creation Options](https://github.com/nirw-snyk/sbom-demo/blob/main/SBOM-Creation-Options.md) 

### Part 2: Testing SBOMs options
To start testing SBOM files follow the steps described in [SBOM Testing Options](https://github.com/nirw-snyk/sbom-demo/blob/main/SBOM-Testing-Options.md) 

### Part 3: Enriching SBOMs options
To start enriching SBOM files follow the steps described in [SBOM Enriching Options](https://github.com/nirw-snyk/sbom-demo/blob/main/SBOM-Enriching-Options.md) 
