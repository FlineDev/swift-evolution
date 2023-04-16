# Unicode for String Processing (Summary)

* Full Proposal: [SE-0363](https://github.com/apple/swift-evolution/blob/main/proposals/0363-unicode-for-string-processing.md)
* Summary Author: [Cihat Gündüz](https://fline.dev/about)
* Article: [Swift Evolution Monthly: July ‘22](https://www.fline.dev/swift-evolution-monthly-july-22/#se-0363-unicode-for-string-processing)

This is the last of the six [Regex String-Processing](https://github.com/apple/swift-experimental-string-processing/blob/main/Documentation/Evolution/ProposalOverview.md?ref=fline.dev) proposals that were explored with the [swift-experimental-string-processing](https://github.com/apple/swift-experimental-string-processing?ref=fline.dev) package on GitHub before they were turned into a set of proposals. I covered them all in past issues ([1](https://www.fline.dev/swift-evolution-monthly-april-22/#se-0350-regex-type-and-overview), [2](https://www.fline.dev/swift-evolution-monthly-april-22/#se-0351-regex-builder-dsl), [3](https://www.fline.dev/swift-evolution-monthly-may-22/#se-0354-regex-literals), [4](https://www.fline.dev/swift-evolution-monthly-may-22/#se-0355-regex-syntax-and-run-time-construction), [5](https://www.fline.dev/swift-evolution-monthly-may-22/#se-0357-regex-powered-string-processing-algorithms)) and they were all accepted already. Some of them have shipped as part of the Xcode 14 betas, it's worth mentioning this year's WWDC sessions [Meet Swift Regex](https://developer.apple.com/videos/play/wwdc2022/110357?ref=fline.dev) & [Swift Regex: Beyond the basics](https://developer.apple.com/videos/play/wwdc2022/110358?ref=fline.dev) again as they provide a great introduction to the topic.

This last proposal explains all the details of how Swifts Regex engine supports Unicode by default, which is not the case for many Regex engines. It also goes into the details of how character classes (which can be thought of as a Swift-native version of [Foundations `CharacterSet` class](https://developer.apple.com/documentation/foundation/characterset?ref=fline.dev)) such as `.digit`, `.whitespace` and `.wordCharacter` are defined and how you can adjust them to fit your needs.

Here are a few key points worth pointing out:

- Swift will do "[grapheme cluster](https://docs.swift.org/swift-book/LanguageGuide/StringsAndCharacters.html?ref=fline.dev#ID296) matching" by default in its Regex engine – this means that the Regex `Caf.` (the `.` matching "any" single character) will match `Café` always correctly – unlike e.g. C#, Rust, Go, Python, Objective-C, Java[](https://docs.oracle.com/javase/7/docs/api/java/util/regex/Pattern.html?ref=fline.dev#CANON_EQ), Ruby, and Perl which would just match `Cafe` without the [acute accent](https://en.wikipedia.org/wiki/Acute_accent?ref=fline.dev) on top of the "e" for an `é` that consists of multiple Unicode scalars ( `"\u{65}\u{301}"`).
- If you need "Unicode scalar" level matching semantics (e.g. for compatibility):

```Swift
"Café".contains(/Café/.matchingSemantics(.unicodeScalar)) // => false
```

- By default, the grouping of characters into character classes follows the modern [Unicode Technical Standard #18](https://unicode.org/reports/tr18/?ref=fline.dev). But custom matching behaviors can be provided, such as `.ignoresCase()`, `dotMatchesNewlines()`, `anchorsMatchNewlines()`, and `asciiOnlyClasses()`. Advanced Regex users might find `[defaultRepetitionBehavior()](https://github.com/apple/swift-evolution/blob/main/proposals/0363-unicode-for-string-processing.md?ref=fline.dev#eagerreluctant-toggle)` very handy.
- The available character classes are: `.any` (= `.`), `.anyNonNewline` (= `.`), `.anyGraphemeCluster` (= `\X`), `.digit` (= `\d`), `.hexDigit`, `.word` (= `\w`), `.whitespace` (= `\s`), `.horizontalWhitespace`(= `\h`), `.verticalWhitespace` (= `\v`), `.newlineSequence` (= `\n` or `\R`).
- Character classes can be inverted, for example:`.digit.inverted` (= `\D`) and `.word.inverted` (= `\W`)
- Like with [Foundations `CharacterSet` class](https://developer.apple.com/documentation/foundation/characterset?ref=fline.dev), you can build a `union` or `intersection` of two character classes or you can be `subtracting`one from another and even build their `symmetricDifference`. You can also specify custom classes with the `.anyOf` and `noneOf` static methods. For example:

```Swift
let floatingNumber: CharacterClass = .digit.union(.anyOf(".,"))
```
