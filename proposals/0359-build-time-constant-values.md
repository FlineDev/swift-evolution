# Build-Time Constant Values (Summary)

* Full Proposal: [SE-0359](https://github.com/apple/swift-evolution/blob/main/proposals/0359-build-time-constant-values.md)
* Summary Author: [Cihat Gündüz](https://fline.dev/about)
* Article: [Swift Evolution Monthly: June ‘22](https://www.fline.dev/swift-evolution-monthly-june-22/#se-0359-build-time-constant-values)

Have you ever found it inconvenient that initializing a `URL` with a clearly valid String literal like `[https://apple.com](https://apple.com/?ref=fline.dev)` returned an `Optional`? I did. I actually always force-unwrap them using by appending `!`. But [no more](https://github.com/apple/swift-evolution/blob/main/proposals/0359-build-time-constant-values.md?ref=fline.dev#enforcement-of-non-failable-initializers)!

This proposal adds a **new `@const` attribute** to type properties, function parameters, and even protocols. This will allow restricting APIs to only accept values that are **known at compile-time** — so basically, we will only be able to pass literal values to them. Future proposals will then be able to add functionality into the toolchain that will allow us to add validity checks at build-time for APIs that are marked with `@const`, preventing us from shipping incorrect code. It also helps the Swift compiler further improve build time.

But that’s not all: This will allow for entirely **new ways of using Swift**, where merely building the code (but never executing it!) provides enough value, such as for the Swift package manifest file, which already has a [pre-pitch](https://forums.swift.org/t/pre-pitch-swiftpm-manifest-based-on-result-builders/53457?ref=fline.dev) to get the same treatment as Regexes have just gotten: A [DSL-style syntax](https://github.com/apple/swift-evolution/blob/main/proposals/0359-build-time-constant-values.md?ref=fline.dev#facilitate-compile-time-extraction-of-values)!

Does that mean we will be able to write a DSL language that can be easily used as a replacement for config file formats like YAML? [Probably](https://forums.swift.org/t/se-0359-build-time-constant-values/57562/75?ref=fline.dev). The example mentioned in the proposal is a “build-time extractable database schema”. This is how a new Swift persistence framework could work. 😍
