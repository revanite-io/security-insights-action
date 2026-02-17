# GitHub Action for Security Insights Validation

This repo provides a GitHub Action for validating a repository's `security-insights.yml` file against the official [OSSF Security Insights](https://github.com/ossf/security-insights) CUE schema. The schema version is extracted from the file itself, so the correct spec version is always used automatically.

## Features

- Validates `security-insights.yml` against the official OSSF CUE schema
- Automatically detects the schema version declared in the file
- Fails loudly on schema mismatches, missing fields, or invalid versions
- No configuration required for the default file location

## Usage

### Basic Example

```yaml
name: Validate Security Insights

on:
  pull_request:
    paths:
      - ".github/security-insights.yml"

permissions:
  contents: read

jobs:
  validate:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v6
        with:
          persist-credentials: false

      - name: Validate Security Insights
        uses: revanite-io/security-insights-action@v1.0.0
```

### Custom File Path

```yaml
      - name: Validate Security Insights
        uses: revanite-io/security-insights-action@v1.0.0
        with:
          file: path/to/security-insights.yml
```

### Inputs

| Input | Description | Required | Default |
|-------|-------------|----------|---------|
| `file` | Path to the `security-insights.yml` file | No | `.github/security-insights.yml` |

## Requirements

- The runner must have `yq` available (pre-installed on `ubuntu-latest`)
- The `schema-version` declared in your `security-insights.yml` must correspond to a valid tag in the [ossf/security-insights](https://github.com/ossf/security-insights) repository (e.g., `2.2.0` resolves to tag `v2.2.0`)

## How It Works

1. Sets up [CUE](https://cuelang.org/) on the runner
2. Reads the `schema-version` from the header of your `security-insights.yml`
3. Downloads the matching CUE schema from `ossf/security-insights` at that version tag
4. Runs `cue vet` to validate the file against the schema

## Contributing

Contributions are welcome! Please open an issue or pull request.

## License

This project is licensed under the Apache 2.0 License - see the [LICENSE](LICENSE.md) file for details.

## Related Projects

- [OSSF Security Insights Spec](https://github.com/ossf/security-insights) - The Security Insights specification and schema
- [OSSF SI Tooling](https://github.com/ossf/si-tooling) - Tooling for working with Security Insights files
