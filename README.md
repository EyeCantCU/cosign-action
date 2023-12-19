# cosign-action

These actions exist to further automate the usage of sigstore's 'cosign'.

# Actions

## Sign

The 'sign' action signs the target container using a private key stored as a repository secret. It is the private key generated by 'cosign' when creating a key pair.

Example implementation for GHCR:

```yaml
jobs:
  example:
    runs-on: ubuntu-latest

    permissions: {}

    name: Sign container with Cosign
    steps:
      - name: Sign container
        uses: EyeCantCU/cosign-action/sign@v0.1.1
        with:
          container: ghcr.io/ublue-os/silverblue-main
          registry-token: ${{ secrets.GITHUB_TOKEN }}
          signing-secret: ${{ secrets.SIGNING_SECRET }}
          tags: latest
```

## Verify

The 'verify' action validates the target container's signature via the public key. For example, for Universal Blue, this is the `cosign.pub` file stored in the root of all image repositories.

Example implementation for verifying against a public key:

```yaml
jobs:
  example:
    runs-on: ubuntu-latest

    permissions: {}

    name: Verify container with Cosign
    steps:
      - name: Verify container
        uses: EyeCantCU/cosign-action/verify@v0.1.1
        with:
          container: ghcr.io/ublue-os/silverblue-main:latest
          pubkey: https://raw.githubusercontent.com/ublue-os/main/main/cosign.pub
```

Example implementation for verifying against a certificate:

```yaml
jobs:
  example:
    runs-on: ubuntu-latest

    permissions: {}

    name: Verify container with Cosign
    steps:
      - name: Verify container
        uses: EyeCantCU/cosign-action/verify@v0.1.1
        with:
          container: cgr.dev/chainguard/busybox
          cert-identity: https://github.com/chainguard-images/images/.github/workflows/release.yaml@refs/heads/main
          oidc-issuer: https://token.actions.githubusercontent.com
```
