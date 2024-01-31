# Debian package signing

A GitHub action to sign debian packages.

## Documentation

### Inputs

#### `src-pkg-name`

**Required** The name of the source package, e.g.
`keyman-srcpkg/keyman_17.0.99-1_source.changes`.

#### `bin-pkg-name`

**Required** The name of the common part of the binary packages. The
action will append `*.changes` to select all binary packages. E.g.
`keyman-srcpkg/keyman_17.0.99-1+`.

#### `artifacts-prefix`

**Required** Common prefix of all artifacts, e.g. `keyman-`.

#### `artifacts-result-name`

**Required** The name of the resulting artifacts, e.g. `keyman-signedpkgs`.

#### `gpg-signing-key`

**Required** The base64 encoded private GPG key used to sign the packages.
This can be done with the following command:

```bash
gpg --export-secret-keys YOUR_ID_HERE | base64
```

#### `debsign-keyid`

**Required** The public fingerprint of the GPG key used to sign the packages.

### Example usage

```yml
  deb_signing:
    name: Sign source and binary packages
    needs: [sourcepackage, binary_packages]
    runs-on: ubuntu-latest
    environment: deploy

    steps:
      - name: Sign packages
        uses: sillsdev/gha-deb-signing@v1
        with:
          src-pkg-name: "keyman_${{ needs.sourcepackage.outputs.VERSION }}-1_source.changes"
          bin-pkg-name: "keyman_${{ needs.sourcepackage.outputs.VERSION }}-1+"
          artifacts-prefix: "keyman-"
          artifacts-result-name: "keyman-signedpkgs"
```
