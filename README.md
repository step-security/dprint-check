[![StepSecurity Maintained Action](https://raw.githubusercontent.com/step-security/maintained-actions-assets/main/assets/maintained-action-banner.png)](https://docs.stepsecurity.io/actions/stepsecurity-maintained-actions)

# dprint check action

A GitHub Action that enforces code formatting by running `dprint check`. If any file is not formatted correctly, the workflow fails.

## Getting started

Add this action after checking out your repository:

```yml
jobs:
  formatting:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v6
      - uses: step-security/dprint-check@v2
```

> **Tip:** When running a multi-platform matrix, limit this action to Linux — formatting checks are platform-independent and there is no value in repeating them on macOS or Windows.
>
> ```yml
> - uses: step-security/dprint-check@v2
>   if: runner.os == 'Linux'
> ```

## Inputs

### `dprint-version`

The version of dprint to install. Omit this input to always use the latest release.

```yml
- uses: step-security/dprint-check@v2
  with:
    dprint-version: 0.30.3
```

### `config-path`

Path to the dprint configuration file. When not provided, dprint auto-discovers the config (e.g. `dprint.json`) from the repository root.

```yml
- uses: step-security/dprint-check@v2
  with:
    config-path: dprint-ci.json
```

### `args`

Any additional flags or arguments to append to the `dprint check` command. For example, to stop at the first formatting error instead of reporting all violations:

```yml
- uses: step-security/dprint-check@v2
  with:
    args: --fail-fast
```

## Troubleshooting

### Line ending errors on Windows

On Windows runners, Git checks out files with CRLF line endings by default. dprint detects this as a formatting difference and reports errors like:

```
from D:\a\check\check\README.md:
 | Text differed by line endings.
--
```

The simplest fix is to run the action on Linux only (see the tip above). If you must run on Windows, force LF line endings before checkout:

```yml
- name: Force LF line endings
  run: |
    git config --global core.autocrlf false
    git config --global core.eol lf

- uses: actions/checkout@v6
```
