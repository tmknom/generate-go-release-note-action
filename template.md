
---

## Installation

Official binaries can be downloaded from ${GITHUB_SERVER_URL}/${GITHUB_REPOSITORY}/releases .
We support popular operating systems such as Linux, Windows, and macOS, as well as CPU architectures like amd64 and arm64.

### Manual Installation

Download the binary for your operating system and architecture, make it executable, and move it to a directory in your `PATH`.
For example, on AMD64 Linux, run the following commands:

```shell
curl -sSL -o ${ARTIFACT_NAME} "${GITHUB_SERVER_URL}/${GITHUB_REPOSITORY}/releases/download/${VERSION}/${ARTIFACT_NAME}_${VERSION}_linux_amd64"
chmod +x ${ARTIFACT_NAME}
sudo mv ${ARTIFACT_NAME} /usr/local/bin/${ARTIFACT_NAME}
```

### Homebrew Installation

```shell
brew install tmknom/tap/${ARTIFACT_NAME}
```

### DEB Installation

Download the `.deb` package for your architecture (e.g., `*_linux_amd64.deb` or `*_linux_arm64.deb`), then install it using the `dpkg` command:

```shell
curl -sSL -o ${ARTIFACT_NAME}.deb "${GITHUB_SERVER_URL}/${GITHUB_REPOSITORY}/releases/download/${VERSION}/${ARTIFACT_NAME}_${VERSION}_linux_amd64.deb"
dpkg -i ${ARTIFACT_NAME}.deb
```

### RPM Installation

Download the `.rpm` package for your architecture (e.g., `*_linux_amd64.rpm` or `*_linux_arm64.rpm`), then install it using the `rpm` command:

```shell
curl -sSL -o ${ARTIFACT_NAME}.rpm "${GITHUB_SERVER_URL}/${GITHUB_REPOSITORY}/releases/download/${VERSION}/${ARTIFACT_NAME}_${VERSION}_linux_amd64.rpm"
rpm -ivh ${ARTIFACT_NAME}.rpm
```

### APK Installation

Download the `.apk` package for your architecture (e.g., `*_linux_amd64.apk` or `*_linux_arm64.apk`), then install it using the `apk` command:

```shell
curl -sSL -o ${ARTIFACT_NAME}.apk "${GITHUB_SERVER_URL}/${GITHUB_REPOSITORY}/releases/download/${VERSION}/${ARTIFACT_NAME}_${VERSION}_linux_amd64.apk"
apk add --allow-untrusted ${ARTIFACT_NAME}.apk
```

## Verification

All artifacts are checksummed, and signed by Cosign using identity-based ("keyless") signing and transparency.

### Checksum verification

1. Install [GitHub CLI](https://cli.github.com/) and [Cosign](https://github.com/sigstore/cosign) if not already available.
2. Download all artifacts using GitHub CLI:
    ```shell
    gh release download ${VERSION_TAG}
    ```
3. Verify the signature using:
    ```shell
    cosign verify-blob \\
      --signature "${ARTIFACT_NAME}_${VERSION}_checksums.txt.sig" \\
      --certificate "${ARTIFACT_NAME}_${VERSION}_checksums.txt.pem" \\
      --certificate-oidc-issuer "${CERTIFICATE_OIDC_ISSUER}" \\
      --certificate-identity "${CERTIFICATE_IDENTITY}" \\
      --certificate-github-workflow-repository "${GITHUB_REPOSITORY}" \\
      --certificate-github-workflow-sha "${GITHUB_SHA}" \\
      "${ARTIFACT_NAME}_${VERSION}_checksums.txt"
    ```
4. Verify the checksum of the downloaded files using:
    ```shell
    sha256sum --ignore-missing -c "${ARTIFACT_NAME}_${VERSION}_checksums.txt"
    ```
    Ensure that the output indicates a valid match.

### Cosign verification

You can verify all artifacts, such as executable binaries, Linux packages, checksum files, and SBOMs.
For example, the `linux/amd64` binary:

1. Install [Cosign](https://github.com/sigstore/cosign) if not already available.
2. Verify the signature using:
    ```shell
    cosign verify-blob \\
      --signature "${ARTIFACT_NAME}_${VERSION}_linux_amd64.sig" \\
      --certificate "${ARTIFACT_NAME}_${VERSION}_linux_amd64.pem" \\
      --certificate-oidc-issuer "${CERTIFICATE_OIDC_ISSUER}" \\
      --certificate-identity "${CERTIFICATE_IDENTITY}" \\
      --certificate-github-workflow-repository "${GITHUB_REPOSITORY}" \\
      --certificate-github-workflow-sha "${GITHUB_SHA}" \\
      "${ARTIFACT_NAME}_${VERSION}_linux_amd64"
    ```

### GitHub Artifact Attestations verification

You can verify all artifacts, such as executable binaries, Linux packages, checksum files, and SBOMs.
For example, the `linux/amd64` binary:

1. Install [GitHub CLI](https://cli.github.com/) if not already available.
2. Verify the attestation:
    ```shell
    gh attestation verify "${ARTIFACT_NAME}_${VERSION}_linux_amd64" \\
      --deny-self-hosted-runners \\
      --repo "${GITHUB_REPOSITORY}" \\
      --cert-oidc-issuer "${CERTIFICATE_OIDC_ISSUER}" \\
      --cert-identity "${CERTIFICATE_IDENTITY}"
    ```

## Container Images

- https://github.com/${GITHUB_REPOSITORY}/pkgs/container/${ARTIFACT_NAME}

### Pull image tag

```shell
docker pull ghcr.io/${GITHUB_REPOSITORY}:${VERSION_TAG}
```

### Pull image digest

```shell
docker pull ghcr.io/${GITHUB_REPOSITORY}@${IMAGE_DIGEST}
```

### Cosign verification

You can verify container images using Cosign.

1. Install [Cosign](https://github.com/sigstore/cosign) if not already available.
2. Verify the signature using:
    ```shell
    cosign verify \\
      --certificate-oidc-issuer "${CERTIFICATE_OIDC_ISSUER}" \\
      --certificate-identity "${CERTIFICATE_IDENTITY}" \\
      --certificate-github-workflow-repository "${GITHUB_REPOSITORY}" \\
      --certificate-github-workflow-sha "${GITHUB_SHA}" \\
      ghcr.io/${GITHUB_REPOSITORY}@${IMAGE_DIGEST}
    ```

### GitHub Artifact Attestations verification

You can verify container images using GitHub Artifact Attestations.

1. Install [GitHub CLI](https://cli.github.com/) if not already available.
2. Verify the attestation:
    ```shell
    gh attestation verify oci://ghcr.io/${GITHUB_REPOSITORY}@${IMAGE_DIGEST} \\
      --deny-self-hosted-runners \\
      --repo "${GITHUB_REPOSITORY}" \\
      --cert-oidc-issuer "${CERTIFICATE_OIDC_ISSUER}" \\
      --cert-identity "${CERTIFICATE_IDENTITY}"
    ```
