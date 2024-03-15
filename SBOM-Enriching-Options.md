# SBOM Enriching Options

### Step 1: install parlay to start enriching SBOM files:
Download and install the relevant distribution for your OS as described [here:](https://github.com/snyk/parlay?tab=readme-ov-file#installation)

Once installed verify you can use parlay:
```
➜  easybuggy git:(demo-feature-branch) ✗ parlay -v
0.4.0%
➜  easybuggy git:(demo-feature-branch) ✗ parlay -h
Enrich an SBOM with context from third party services

Usage:
  parlay
  parlay [command]

Available Commands:
  deps        Commands for using parlay with deps.dev
  ecosystems  Commands for using parlay with ecosystem.ms
  help        Help about any command
  scorecard   Commands for using parlay with OpenSSF Scorecard
  snyk        Commands for using parlay with Snyk

Flags:
      --debug
  -h, --help      help for parlay
  -v, --version   version for parlay

Use "parlay [command] --help" for more information about a command.
```

### Step 2: start enriching:

#### enriching with [ecosyste.ms]:(https://ecosyste.ms/)
This adds information about each package including license details, external links, maintainer information, and more.

```
easybuggy git:(demo-feature-branch) ✗ parlay ecosystems enrich myCyclonedx1.4.json >& myEnrichedCyclonedx1.4.json
```

example file [here](https://github.com/nirw-snyk/sbom-demo/blob/main/samples/myEnrichedCyclonedx1.4.json)
>Note: the raw json output was beautified


#### enriching with vulnerability data from snyk
This adds information about known vulnerabilities for each open source packages 
```
easybuggy git:(demo-feature-branch) ✗ parlay snyk enrich myCyclonedx1.4.json >& mySnykEnrichedCyclonedx1.4.json
```
example file [here](https://github.com/nirw-snyk/sbom-demo/blob/main/samples/mySnykEnrichedCyclonedx1.4.json)
>Note 1: the raw json output was beautified
>Note 2: parlay currently using hardcoded API endpoint - we'll be adding support to use the SNYK_API envieonment variable in the near future.


#### enriching with [OpenSSF Scorecard](https://securityscorecards.dev/)
This will add an external reference to the Scorecard API which can be used to retrieve the full scorecard.
```
  easybuggy git:(demo-feature-branch) ✗ parlay scorecard enrich myCyclonedx1.4.json >& myScoreCardEnrichedCyclonedx1.4.json
```
example file [here](https://github.com/nirw-snyk/sbom-demo/blob/main/samples/myScoreCardEnrichedCyclonedx1.4.json)
>Note: the raw json output was beautified

#### why not using all 3 - pipes!
This will enrich our SBOM with all 3 into one:
```
 easybuggy git:(demo-feature-branch) ✗ cat myCyclonedx1.4.json |  parlay  scorecard enrich - | parlay ecosystems enrich - | parlay snyk enrich - >& myAllInOneEnrichedCyclonedx1.4.json
 ```
example file [here](https://github.com/nirw-snyk/sbom-demo/blob/main/samples/myAllInOneEnrichedCyclonedx1.4.json)
>Note: the raw json output was beautified

###