# Build and Release Rust program using GitHub action

A GitHub workflow that builds rust program on multiple platforms and publish them as release.

## Example workflow

```yaml
on:
  push:
    tags:
      - "**"
jobs:
  call-publish:
    uses: hafeoz/rust-build-release-workflow/.github/workflows/build-and-publish.yaml@master
    permissions:
      contents: write
      id-token: write
      attestations: write
    with:
      fail-if-unreproducible: true
      rust-toolchain: nightly
```

## Options

| Name                   | Type    | Description                                                                                                                                                                                                                                                   | Default                              |
|------------------------|---------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------|
| target-regex           | string  | If specified, only targets following given regex will be built.                                                                                                                                                                                               | ^([^-]+)-([^-]+)-([^-]+)(-([^-]+))?$ |
| skip-target            | string  | Targets that should not build against, as array of strings. By default all tier 1 and 2 rust targets will be built.                                                                                                                                           | []                                   |
| rust-toolchain         | string  | Rust toolchain to use.                                                                                                                                                                                                                                        |                                      |
| rust-flags             | string  | RUSTFLAGS environment variable used when compilation.                                                                                                                                                                                                         |                                      |
| test-reproducibility   | boolean | Whether to check if binary is reproducible.                                                                                                                                                                                                                   | true                                 |
| reprotest-variations   | string  | Variations to test for reproducibility, as comma-separated of strings.                                                                      | +all,-time                           |
| fail-if-unreproducible | boolean | Whether to fail the workflow if binary is unreproducible. By default unreproducible binaries are warned but not failed.                                                                                                                                       | false                                |
| cargo-verbose          | boolean | Whether or not display extra detailed messages for Cargo.                                                                                                                                                                                                     | false                                |
| reprotest-verbose      | number  | Level of verbosity (0~2) for reprotest.                                                                                                                                                                                                                       | 1                                    |
| executable-filter      |         |                                                                                                                                                                                                                                                               |                                      |

## Caveat and security considerations

- Not all `reprotest` variations are enabled by default: `time` cause rustc to SIGSEGV and is likely to interfere with dependency fetching.

- `Cargo.lock` is used to download dependencies from network instead of vendored, which may cause non-determinism and possible supply chain security issue.

## License

This software is licensed under [BSD Zero Clause](https://spdx.org/licenses/0BSD.html) OR [CC0 v1.0 Universal](https://spdx.org/licenses/CC0-1.0.html) OR [WTFPL Version 2](https://spdx.org/licenses/WTFPL.html).
You may choose any of them at your will.
