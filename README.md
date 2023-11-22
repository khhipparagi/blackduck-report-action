# Black Duck report action

GitHub action to produce a SBOM report from a given Black Duck project.

## Problem

When you get your project analyzed in Black Duck, you might also want to be able to create a report in your ci/cd build pipeline.

Black Duck can generate SPDX SBOM, but there is no way of trigger this with the official GitHub Action.

## Purpose of this action

This action will enable you to trigger the creation of a Black Duck report (defaulted to SPDX22).
It will also wait for Black Duck to complete the report and download it.

## Usage

<!-- action-docs-description -->
## Description

Create Black Duck Report and download it
<!-- action-docs-description -->
<!-- action-docs-inputs -->
## Inputs

| parameter | description | required | default |
| --- | --- | --- | --- |
| blackduck-url | url to Black Duck instance | `true` |  |
| blackduck-token | Black Duck API token | `true` |  |
| project | Project name in Black Duck | `true` |  |
| version | Version in Black Duck | `true` |  |
| report-format | sbomType "SPDX_22" allows reportFormat values of "JSON", "RDF", "TAGVALUE" or "YAML". sbomType "CYCLONEDX_13" or "CYCLONEDX_14" allows reportFormat values of "JSON". sbomType "VERSION_LICENSE" allows reportFormat value "TEXT". | `false` | JSON |
| sbom-type | Type of SBOM report. Allowed values - SPDX_22, CYCLONEDX_13, CYCLONEDX_14, or VERSION_LICENSE | `false` | SPDX_22 |
<!-- action-docs-inputs -->
<!-- action-docs-outputs -->
## Outputs

| parameter | description |
| --- | --- |
| sbom-file | SBOM filename if created |
| sbom-contents | SBOM content if created |
<!-- action-docs-outputs -->

## Example usage

```yaml
- uses: philips-software/blackduck-report-action@v0.3
  id: blackduck-report
  with:
    blackduck-url: https://my-blackduck-server
    blackduck-token: ${{ secrets.BLACKDUCK_TOKEN }}
    project: my-project
    version: my-version

- name: show content - Be careful... sboms are huge.. this might cause some problems with io on GitHub.
  run: echo ${{steps.blackduck-report.outputs.sbom-contents}}

- name: Upload artifact
  uses: actions/upload-artifact@3cea5372237819ed00197afe530f5a7ea3e805c8
  with:
    name: sbom-report
    path: ${{steps.blackduck-report.outputs.sbom-file}}
    retention-days: 7
```

### Script only

```bash
./get-blackduck-report.sh <blackduck-url> <blackduck-api-token> <project-name> <version-name>
```

## Example

[Here](https://github.com/philips-software/blackduck-report-action/blob/main/CONTRIBUTING.md#example-workflow) you can find an example of a complete workflow including the scanning of a project.

## Contributing

You are welcome to contribute to this repository. Please look in [the contributing guide](./CONTRIBUTING.md) how to do this.

## Maintainers

[Here](./MAINTAINERS.md) you can find the maintainers of this project.

## License

MIT

## SBOM

This action only generates an SBOM report in Black Duck and downloads it. The report is not necessarily providing the correct SBOM.

<img src="./.github/assets/code-slogan.svg" align="right" width="450px">


