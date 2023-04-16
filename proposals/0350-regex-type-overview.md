# Regex Type and Overview (Summary)

* Full Proposal: [SE-0350](https://github.com/apple/swift-evolution/blob/main/proposals/0350-regex-type-overview.md)
* Summary Author: [Cihat Gündüz](https://fline.dev/about)
* Article: [Swift Evolution Monthly: April ‘22](https://www.fline.dev/swift-evolution-monthly-april-22/#se-0350-regex-type-and-overview)

This is a big one. Not necessarily this proposal specifically. But it is kicking off a [series of Regex-related proposals](https://github.com/apple/swift-experimental-string-processing/blob/main/Documentation/Evolution/ProposalOverview.md?ref=fline.dev) that are going to greatly improve how we can process Strings to read/transform specific data from parts of them. To put things in context, this is the start to implement [Declarative String Processing](https://github.com/apple/swift-experimental-string-processing/blob/main/Documentation/DeclarativeStringProcessing.md?ref=fline.dev) in Swift, which itself is part of a larger [data processing improvement](https://github.com/apple/swift-experimental-string-processing/blob/main/Documentation/BigPicture.md?ref=fline.dev) plan.

But let’s get back to this proposal in particular. It introduces a `Regex` type:

```Swift
let pattern = #"(\w+)@(.*)"#
let anyRegex = try! Regex (compiling: pattern) // -> Regex<AnyRegexOutput>

let typedRegex: Regex<(Substring, Substring, Substring)> = try! Regex(compiling: pattern)
```

Note that the first `Substring` always contains the full match.

As you can see, unlike `NSRegularExpression`, the new `Regex` type will be generic over its output, allowing for more type safety when dealing with Regexes. The `Regex` type will ship with a whole set of functions, such as `firstMatch(in:)`returning a `Regex<Output>.Match?`, which is a wrapper for the matched `range` and the typed `output`.
