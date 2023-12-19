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
- uses: uses: Mobelux/bump-version-action@v1
  id: bump
  with:
    version: `1.0.0'
    release-type: 'minor'
```

See this repo's [release workflow](.github/workflows/release.yml) for a complete example.

## Acknowledgments

Uses [semver-tool](https://github.com/fsaintjacques/semver-tool)
