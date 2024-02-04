# status-report-action
[![Release](https://github.com/infrastructure-blocks/status-report-action/actions/workflows/git-tag-semver-from-label.yml/badge.svg)](https://github.com/infrastructure-blocks/status-report-action/actions/workflows/git-tag-semver-from-label.yml)
[![Self Test](https://github.com/infrastructure-blocks/status-report-action/actions/workflows/self-test.yml/badge.svg)](https://github.com/infrastructure-blocks/status-report-action/actions/workflows/self-test.yml)
[![Update From Template](https://github.com/infrastructure-blocks/status-report-action/actions/workflows/update-from-template.yml/badge.svg)](https://github.com/infrastructure-blocks/status-report-action/actions/workflows/update-from-template.yml)

A simple action to provide a status report as an issue comment. The status report is edited in place on every
update. There is also an option to simply remove the report, if present.

It provides a certain format and makes certain assumptions. It assumes the report is coming from an
action or a workflow. The header of the report is the name of the action's repository, and it is linking
to the repository's code (assumed to be hosted on GitHub),

Because the action could be invoked several times in a given workflow, depending on execution conditions,
the repository input can also be passed through the `env` context. See usage below.

## Inputs

|     Name     | Required | Description                                                                                                                                                                                                                                                                                                                                                   |
|:------------:|:--------:|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     mode     |  false   | The mode of report action. This can either be "clear" or "upsert". When "clear" is provided, the existing report is deleted, if it exists. It doesn't fail in the case that the report is missing. When "upsert" is provided, the `body` input must be provided and the comment is created if it doesn't exist or updated otherwise. It defaults to "upsert". |
|     body     |  false   | The report content. This is required when `mode` is "upsert", irrelevant otherwise.                                                                                                                                                                                                                                                                           |
| issue-number |  false   | The issue where to post the comment. This defaults to the PR number on pull request events, otherwise, it should be provided.                                                                                                                                                                                                                                 |
|  repository  |  false   | The repository of the action that is publishing the report. Defaults to ${{ env.status-report-action-repository. }}                                                                                                                                                                                                                                           |

## Outputs

|    Name    | Description                                                                                    |
|:----------:|------------------------------------------------------------------------------------------------|
| comment-id | The comment ID of the status report. When `mode` "clear" is selected, this is an empty string. |

## Permissions

One of the below permissions should be provided, depending on if the status report will show on a pull request
or on an issue.

|     Scope     | Level | Reason                                      |
|:-------------:|:-----:|---------------------------------------------|
| pull-requests | write | Required to post comments on pull-requests. |
|    issues     | write | Required to post comments on issues.        |

## Usage

### With input
```yaml
name: Status Report

on:
  pull-request: ~

permissions:
  pull-requests: write

jobs:
  status-report:
    steps:
      - uses: infrastructure-blocks/status-report-action@v1
        with:
          repository: captain-shitcoin/shitcoin-repo
          body: |
            :+1: My shitcoin is good!
```

### With env
```yaml
name: Status Report

on:
  pull-request: ~

permissions:
  pull-requests: write

env:
  status-report-action-repository: captain-shitcoin/shitcoin-repo

jobs:
  status-report:
    steps:
      - if: ${{ 'true' == 'false' }}
        uses: infrastructure-blocks/status-report-action@v1
        with:
          body: |
            :+1: My shitcoin is good!
      - if: ${{ 'true' == 'true' }}
        uses: infrastructure-blocks/status-report-action@v1
        with:
          body: |
            :disappointed:  My shitcoin is not good :disappointed:
```

### Explicit issue-number
```yaml
name: Status Report

on:
  push: ~

permissions:
  pull-requests: write

jobs:
  status-report:
    steps:
      # Gets the current PR associated with GITHUB_SHA
      - id: get-current-pr
        uses: 8BitJonny/gh-get-current-pr@2.2.0
      - uses: infrastructure-blocks/status-report-action@v1
        with:
          issue-number: ${{ steps.get-current-pr.outputs.number }}
          repository: captain-shitcoin/shitcoin-repo
          body: |
            :+1: My shitcoin is when there is a PR!
```

## Development

### Releasing

The releasing is handled at git level with semantic versioning tags. Those are automatically generated and managed
by the [git-tag-semver-from-label-workflow](https://github.com/infrastructure-blocks/git-tag-semver-from-label-workflow).
