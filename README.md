# Debian package signing

A GitHub action to sign debian packages.

## Documentation

### Inputs

#### `src-pkg-path`

**Required** The path that contains the source package files, e.g.
`artifacts/keyman-srcpkg`.

#### `src-pkg-name`

**Required** The name of the source package, e.g.
`keyman-srcpkg/keyman_17.0.99-1_source.changes`.

#### `bin-pkg-path`

**Required** The path that contains the binary package files, e.g.
`artifacts/keyman-binarypkgs`.

#### `bin-pkg-name`

**Required** The name of the common part of the binary packages. The
action will append `*.changes` to select all binary packages. E.g.
`keyman-srcpkg/keyman_17.0.99-1+`.

#### `artifacts-name`

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
          src-pkg-path: "artifacts/keyman-srcpkg"
          src-pkg-name: "keyman_${{ needs.sourcepackage.outputs.VERSION }}-1_source.changes"
          bin-pkg-path: "artifacts/keyman-binarypkgs"
          bin-pkg-name: "keyman_${{ needs.sourcepackage.outputs.VERSION }}-1+"
          artifacts-name: "keyman-signedpkgs"
```
