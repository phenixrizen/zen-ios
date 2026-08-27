# Contributing

This is the Swift package for [`phenixrizen/zen`](https://github.com/phenixrizen/zen), a
maintained fork of `gorules/zen`. Maintained by Phenix Rizen (Nathan Rockhold).

## This repository is a distribution package

`ZenUniffi.xcframework` and `Sources/` are **build artifacts**, not source. They are produced by
the `UniFFI` workflow in `phenixrizen/zen` and pushed here as a "chore: update XCFramework"
branch. Do not hand-edit them — change the engine and let the pipeline rebuild.

| change | repository |
| --- | --- |
| Evaluation, expressions, node kinds | [`phenixrizen/zen`](https://github.com/phenixrizen/zen) |
| The Swift API surface / UniFFI bindings | `phenixrizen/zen` under `bindings/uniffi` |
| `Package.swift`, platform targets, packaging | here |

## Using it

```swift
.package(url: "https://github.com/phenixrizen/zen-ios", from: "2.0.0")
```

## Building

```bash
swift build
```

Requires Xcode 15 or later; the package targets iOS 16+.

## Commit messages

[Conventional Commits](https://www.conventionalcommits.org/).

## Security

Do not open a public issue for a security problem — see [SECURITY.md](.github/SECURITY.md).
