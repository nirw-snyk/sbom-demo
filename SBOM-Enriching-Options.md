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

```
easybuggy git:(demo-feature-branch) ✗ parlay ecosystems enrich myCyclonedx1.4.json >& myEnrichedCyclonedx1.4.json
```

example file [here](https://github.com/nirw-snyk/sbom-demo/blob/main/samples/myEnrichedCyclonedx1.4.json)
>Note: the raw json output was beautified


###