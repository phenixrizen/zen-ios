> [!NOTE]
> ## This is a maintained fork
>
> **`phenixrizen/zen-ios` is the Swift package for [`phenixrizen/zen`](https://github.com/phenixrizen/zen), a maintained fork of `gorules/zen`, maintained by Phenix Rizen (Nathan Rockhold).**
>
> The bundled XCFramework is built from that fork, so it carries fixes upstream does not have:
> decision-level `$params`, `TZ`-aware date resolution, and a fix for silent truncation of
> fractional numbers — where every non-integer value was rounded down before serialization.
>
> The fork's `databaseNode` is present in the engine but **not usable from Swift**: the UniFFI
> binding links no database handler and exposes no way to register one, so evaluating a
> `databaseNode` returns "Database handler not provided". It is available in the Rust, Go and
> Node.js bindings.
>
> Add it by URL:
>
> ```swift
> .package(url: "https://github.com/phenixrizen/zen-ios", from: "2.1.0")
> ```
>
> Not affiliated with or endorsed by GoRules. MIT licensed, same as upstream, with the original
> copyright retained in [LICENSE](LICENSE).

# Swift Rules Engine for iOS

**Business logic humans can read and machines can run.** One copy of your rules: the owner reads it, every system runs it.

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)

ZEN Engine is a cross-platform, open-source Business Rules Engine (BRE) written in **Rust**, packaged here as a **Swift** package with a precompiled XCFramework for **iOS**. Decisions evaluate in microseconds, on-device and offline-capable, and are stored as portable JSON that runs identically on every platform: the same rules power Node.js, Python, Go, Java, Kotlin and .NET backends.

## Rules that read like sentences

Conditions are written the way the business says them, in the ZEN Expression Language. The developer view is one toggle away, and the two can never drift apart: there is only one source of truth, and this engine runs it.

## Rules as graphs, or as documents

Model a decision on a visual canvas of decision tables, switches, expressions, functions and reusable sub-decisions. Or write it as a policy document with prose, typed data models and tables. Both compile to the same engine and return the same answers.

A JDM document is either a **graph** (decision tables, switches, expressions, functions and reusable sub-decisions) or a **policy** (prose, typed data models and tables). Both compile to the same engine and return the same answers.

## Installation

Add the package in Xcode via **File → Add Package Dependencies...** with the repository URL `https://github.com/phenixrizen/zen-ios`, or in `Package.swift`:

```swift
dependencies: [
    .package(url: "https://github.com/phenixrizen/zen-ios", from: "2.1.0")
]
```

Then add `ZenUniffi` to your target dependencies:

```swift
.target(
    name: "YourTarget",
    dependencies: [
        .product(name: "ZenUniffi", package: "zen-ios")
    ]
)
```

## Quickstart

```swift
import ZenUniffi

Task {
    guard let ruleData = Bundle.main.url(forResource: "pricing", withExtension: "json")
        .flatMap({ try? Data(contentsOf: $0) }) else {
        return
    }

    let engine = try ZenEngine(loader: nil, customNode: nil)
    let decision = try engine.createDecision(content: ruleData)

    let input = """
        {
            "customer": { "tier": "gold", "yearsActive": 3 },
            "order": { "subtotal": 150, "items": 5 }
        }
    """.data(using: .utf8)!

    let response = try await decision.evaluate(context: input, options: nil)

    if let resultString = String(data: response.result, encoding: .utf8) {
        print(resultString)
        // => {"discount":0.15,"freeShipping":true}
    }
}
```

### Loaders

`ZenEngine` accepts an optional `ZenLoader` that serves decisions by key. Use the `static`, `filesystem`, or `zip` variants for common backends, or `callback` for custom loading logic. With a configuration, decisions are pre-loaded and pre-compiled at engine creation for faster evaluations.

```swift
import ZenUniffi
import Foundation

func createEngine() throws -> ZenEngine {
    let url = Bundle.main.url(forResource: "pricing", withExtension: "json")!
    let pricing = try Data(contentsOf: url)

    let loader = ZenLoader.static(content: ["pricing.json": pricing])
    return try ZenEngine(loader: loader, customNode: nil)
}
```



## Other platforms

* **Node.js** — [npm](https://www.npmjs.com/package/@phenixrizen/zen-engine)
* **Python** — [PyPI](https://pypi.org/project/phenixrizen-zen-engine/)
* **Go** — [phenixrizen/zen-go](https://github.com/phenixrizen/zen-go)
* **Java / Kotlin / Android** — [source](https://github.com/phenixrizen/zen/tree/master/bindings/uniffi)
* **.NET** — [NuGet](https://www.nuget.org/packages/PhenixRizen.ZenEngine)
* **Rust (core)** — [phenixrizen/zen](https://github.com/phenixrizen/zen) | [crates.io](https://crates.io/crates/phenixrizen-zen-engine)


## Requirements

- iOS 16.0+
- Swift 5.9+

## Contribution

**Contributions are welcome here.** This fork exists partly because upstream cannot take them.

Note that this repository is a distribution package, not source: `ZenUniffi.xcframework` and
`Sources/` are build artifacts produced by the `UniFFI` workflow in
[`phenixrizen/zen`](https://github.com/phenixrizen/zen) and pushed here. A change to evaluation
or to the Swift bindings belongs there; only packaging changes belong here.

## License

[MIT License](https://opensource.org/licenses/MIT)

## Support

For issues and questions, please visit [github.com/phenixrizen/zen-ios/issues](https://github.com/phenixrizen/zen-ios/issues).
