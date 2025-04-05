# generate-go-release-note-action

Generates a release note for Go.

<!-- actdocs start -->

## Description

This action generates structured, ready-to-use release notes for Go projects.
The notes are provided in Markdown format and include installation instructions, artifact verification steps, and examples for using container images.

## Usage

```yaml
  steps:
    - name: Generate Go Release Note
      uses: tmknom/generate-go-release-note-action@v0
      with:
        artifact-name: pseudo
        version-tag: v1.2.3
        image-digest: sha256:abc123
        release-workflow-ref: tmknom/release-workflows/.github/workflows/go.yml@abc123
```

## Inputs

| Name | Description | Default | Required |
| :--- | :---------- | :------ | :------: |
| artifact-name | The name of the release artifact, typically the executable binary or container image name. | n/a | yes |
| image-digest | The digest (algorithm:hex) of the released container image (e.g., `sha256:abc123`). | n/a | yes |
| release-workflow-ref | The full GitHub reference (owner/repo/path@ref) to the release workflow (e.g., `tmknom/release-workflows/.github/workflows/go.yml@abc123`). | n/a | yes |
| version-tag | The GitHub release version tag in semantic versioning format (e.g., `v1.2.3`). | n/a | yes |

## Outputs

| Name | Description |
| :--- | :---------- |
| release-note-path | The filepath to the generated release note in Markdown format. |

<!-- actdocs end -->

## Permissions

N/A

## FAQ

N/A

## Related projects

N/A

## Release notes

See [GitHub Releases][releases].

[releases]: https://github.com/tmknom/generate-go-release-note-action/releases
