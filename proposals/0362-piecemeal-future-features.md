# Piecemeal adoption of upcoming language improvements (Summary)

* Full Proposal: [SE-0362](https://github.com/apple/swift-evolution/blob/main/proposals/0362-piecemeal-future-features.md)
* Summary Author: [Cihat Gündüz](https://fline.dev/about)
* Article: [Swift Evolution Monthly: June ‘22](https://www.fline.dev/swift-evolution-monthly-june-22/#se-0362-piecemeal-adoption-of-future-language-improvements)

Source-breaking changes in Swift are handled with great caution. So much so, that some new language features are already implemented and shipped with versions of Swift 4/5, but are disabled because they are marked as “only available in Swift 6 language mode”. Some of them shipped with a custom feature flag to turn them on today but even they are hard to explore.

This proposal streamlines that by adding the feature flag specifier `--enable-future-feature` and requiring all future & even past proposals to adopt this by specifying their own feature identifier for it. This way developers have a unified way to enable and adopt select new features at their own pace.

This not only gives you the benefit of a Swift 6 improvement today but also allows you to easily prepare for Swift 6 on a step-by-step basis. I encourage you to turn on each feature one by one to see its impact to your code base as soon as the unified flags are available. This way you can discuss each feature with your team and create a roadmap for when to enable them to smoothen your transition to Swift 6 proactively.

Thanks to a new `#if hasFeature()` check, it will even be possible to keep two versions of the same code to not have to change your code when enabling/disabling a feature or simply to support the integration of your framework into tools where the feature is not available. For example:

```Swift
#if compiler(>=5.7) && hasFeature(BareSlashRegexLiterals)
	let regex = /.../
#else
	let regex = try NSRegularExpression(pattern: "...")
#endif
```

The adoption of specific features will also be possible by specifying an array of feature identifiers to a `target` in SwiftPM manifest files like so:

```Swift
.target(
	name: "myPackage",
	futureFeatures: ["ConciseMagicFile", "ExistentialAny"]
)
```

Here’s a list of all initial feature identifiers that are planned to be supported:

- [SE-0274](https://github.com/apple/swift-evolution/blob/main/proposals/0274-magic-file.md?ref=fline.dev): `ConciseMagicFile`
- [SE-0286](https://github.com/apple/swift-evolution/blob/main/proposals/0286-forward-scan-trailing-closures.md?ref=fline.dev): `ForwardTrailingClosures`
- [SE-0335](https://github.com/apple/swift-evolution/blob/main/proposals/0335-existential-any.md?ref=fline.dev): `ExistentialAny`
- [SE-0337](https://github.com/apple/swift-evolution/blob/main/proposals/0337-support-incremental-migration-to-concurrency-checking.md?ref=fline.dev): `StrictConcurrency`
- [SE-0352](https://github.com/apple/swift-evolution/blob/main/proposals/0352-implicit-open-existentials.md?ref=fline.dev): `ImplicitOpenExistentials` (covered in [April Issue](https://se-monthly.flinedev.com/issues/swift-evolution-monthly-april-22-regex-overhaul-improved-existentials-swift-5-7-timeline-1093738?ref=fline.dev))
- [SE-0354](https://github.com/apple/swift-evolution/blob/main/proposals/0354-regex-literals.md?ref=fline.dev): `BareSlashRegexLiterals` (covered in [May Issue](https://se-monthly.flinedev.com/issues/swift-evolution-monthly-may-22-regex-overhaul-pt-ii-swift-snippets-new-workgroups-1146132?ref=fline.dev))
