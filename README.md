# composite-action-template

A simple action to provide a status report as a PR comment. The status report is edited in place on every
update.

## Usage

The defaults will infer the title and report link from the calling repository. Those options can be tweaked
through inputs, however. See [action.yml](./action.yml).
```yaml
permissions:
  pull-requests: write

jobs:
  some-job:
    steps:
      - uses: infrastructure-blocks/status-report-action@v1
        with:
          body: |
            :+1:  **Notice**: found "${{ steps.check-labels.outputs.matched-labels }}" label, understood. Will be tagging release branch ${{ github.base_ref }} upon merge.
```

## Development

### Releasing

The releasing is handled at git level with semantic versioning tags. Those are automatically generated and managed
by the [git-tag-semver-from-label-workflow](https://github.com/infrastructure-blocks/git-tag-semver-from-label-workflow).
