# status-report-action

A simple action to provide a status report as a PR comment. The status report is edited in place on every
update.

It provides a certain format and makes certain assumptions. It assumes the report is coming from an
action or a workflow. The header of the report is the name of the action's repository, and it is linking
to the repository's code (assumed to be hosted on GitHub),

Because the action could be invoked several times in a given workflow, depending on execution conditions,
the repository input can also be passed through the `env` context. See usage below.

## Usage

### With input
```yaml
permissions:
  pull-requests: write

jobs:
  some-job:
    steps:
      - uses: infrastructure-blocks/status-report-action@v1
        with:
          repository: captain-shitcoin/shitcoin-repo
          body: |
            :+1: My shitcoin is good!
```
### With env
```yaml
permissions:
  pull-requests: write

env:
  status-report-action-repository: captain-shitcoin/shitcoin-repo

jobs:
  some-job:
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
## Development

### Releasing

The releasing is handled at git level with semantic versioning tags. Those are automatically generated and managed
by the [git-tag-semver-from-label-workflow](https://github.com/infrastructure-blocks/git-tag-semver-from-label-workflow).
