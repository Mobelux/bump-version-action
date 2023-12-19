# Bump Version

A GitHub Action to bump a given version number.

## Inputs

```yaml
inputs:
  version:
    description: The current semantic version number.
    required: true
  release-type:
    description: Type of release. Must be one of `patch` | `minor` | `major`.
    required: true
```

## Outputs

```yaml
outputs:
  version:
    description: The new version number.
  major-version:
    description: The major version number.
```

## Example Usage

```yaml
name: Bump Version
on:
  workflow_dispatch:
    inputs:
      release_type:
        description: Type of release
        type: choice
        required: true
        options:
          - patch
          - minor
          - major
jobs:
  release:
    runs-on: ubuntu-latest
    permissions:
      contents: write
    steps:
      - uses: actions/checkout@v4
      - uses: actions/github-script@v7
        id: get_current_version
        with:
          script: |
            const { data: { tag_name } } = await github.rest.repos.getLatestRelease({
              owner: context.repo.owner,
              repo: context.repo.repo
            })
            return tag_name
      - uses: uses: Mobelux/bump-version-action@v1
        id: bump
        with:
          version: ${{ steps.get_current_version.outputs.result }}
          release-type: ${{ inputs.release_type }}
      - uses: actions/github-script@v7
        with:
          script: |
            github.rest.git.createRef({
              owner: context.repo.owner,
              repo: context.repo.repo,
              ref: 'refs/tags/${{ steps.bump.outputs.version }}',
              sha: context.sha
            })
```

## Acknowledgments

Uses [semver-tool](https://github.com/fsaintjacques/semver-tool)
