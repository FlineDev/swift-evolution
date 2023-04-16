# `buildPartialBlock` for result builders (Summary)

* Full Proposal: [SE-0348](https://github.com/apple/swift-evolution/blob/main/proposals/0348-buildpartialblock.md)
* Summary Author: [Cihat Gündüz](https://fline.dev/about)
* Article: [Swift Evolution Monthly: April ‘22](https://www.fline.dev/swift-evolution-monthly-april-22/#se-0348-buildpartialblock-for-result-builders)

This one will only concern you *directly* if you try [writing a DSL using Result Builders](https://developer.apple.com/videos/play/wwdc2021/10253/?ref=fline.dev). But even as a user of frameworks like SwiftUI or Point-Free’s [swift-parsing](https://github.com/pointfreeco/swift-parsing?ref=fline.dev), you are going to benefit from this *indirectly* because it **significantly reduces the number of overloads** the library authors have to write to make things like `VStack` support multiple subviews.

This should allow Apple to accept **more than 10 views** in the future, making [workarounds like `Group`](https://stackoverflow.com/a/58398355?ref=fline.dev) less necessary. Point-Free has [already reported](https://forums.swift.org/t/pitch-buildpartialblock-for-result-builders/55561/10?ref=fline.dev) that they will be able to increase their parser builder limit from 6 to above 10 and even to infinity for some specific combination of types. Cross-platform SwiftUI implementations such as [Tokamak](https://github.com/TokamakUI/Tokamak?ref=fline.dev) or [Swift Cross UI](https://github.com/stackotter/swift-cross-ui?ref=fline.dev) should profit from this, too. It’s also going to allow for new types of builders, such as the new [`RegexComponentBuilder`](https://github.com/apple/swift-experimental-string-processing/blob/85c7d906dd871364357156126278d9d427936ca4/Sources/_StringProcessing/RegexDSL/Builder.swift?ref=fline.dev#L13) planned for the Regex proposals below.

The feature is already implemented and will ship with Swift `5.7`.
