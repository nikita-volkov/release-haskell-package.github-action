# Summary

Github Action which automates the release process for Haskell open-source libraries. It manages versions by bumping them in the Cabal-file, attempts to publish the package on Hackage, and if successful merges the updated version to your master branch and marks the released commit with the version tag.

This action promotes an approach to CI/CD where you use pushes to release branches to signal an automated release and use master/main-branch for development.

# Examples

Following is an example of a workflow that gets triggered by pushes to one of the release branches. The name of the release branch determines which version place to bump.

```yaml
name: Release the lib to Hackage

on:
  push:
    branches:
      - supermajor
      - major
      - minor
      - patch

concurrency:
  group: release
  cancel-in-progress: false

jobs:
  release:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: nikita-volkov/release-haskell-package.github-action@v1.0.0
        with:
          hackage-token: ${{ secrets.HACKAGE_TOKEN }}
          version-bump-place: ${{ fromJSON('{"supermajor":0,"major":1,"minor":2,"patch":3}')[github.ref_name] }}
          main-branch: master
```
