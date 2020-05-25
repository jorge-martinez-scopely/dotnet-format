# GitHub Action for dotnet-format

[![CI Workflow Status](https://github.com/xt0rted/dotnet-format/workflows/CI/badge.svg)](https://github.com/xt0rted/dotnet-format/actions?query=workflow%3ACI)
[![Dependabot Status](https://api.dependabot.com/badges/status?host=github&repo=xt0rted/dotnet-format)](https://dependabot.com)

Run [dotnet-format](https://github.com/dotnet/format) as part of your workflow to report formatting errors or auto fix violations as part of your pull request workflow.

## Usage

Running on `pull_request`.

```yml
name: Format check on pull request
on: pull_request
jobs:
  dotnet-format:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout repo
        uses: actions/checkout@v2

      - name: Add dotnet-format problem matcher
        uses: xt0rted/dotnet-format-problem-matcher@v1

      - name: Restore dotnet tools
        uses: xt0rted/dotnet-tool-restore@v1

      - name: Run dotnet format
        id: format
        uses: victor-alcazar/dotnet-format@v1.0.2
        with:
          repo-token: ${{ secrets.GITHUB_TOKEN }}
          action: "fix"
          only-changed-files: false
          folder: "src"

```

## Options

### Required

Name | Allowed values | Description
-- | -- | --
`repo-token` | `GITHUB_TOKEN` or a custom value | The token used to call the GitHub api.

### Optional

Name | Allowed values | Description
-- | -- | --
`action` | `check` (default), `fix` | The primary action dotnet-format should perform.
`only-changed-files` | `true`, `false` (default) | Only changed files in the current pull request should be formatted.
`fail-fast` | `true` (default), `false` | The job should fail if there's a formatting error. Only used with the `check` action.
`workspace` | `.` | The solution or project file to operate on.
`folder` | `.` | The folder to operate on. Cannot be used with the `--workspace` option.

## Outputs

Name | Description
-- | --
`has-changes` | If any files were found to have violations or had fixes applied. Will be a string value of `true` or `false`.

## License

The scripts and documentation in this project are released under the [MIT License](LICENSE)
