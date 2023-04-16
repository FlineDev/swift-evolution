# Regex-powered string processing algorithms (Summary)

* Full Proposal: [SE-0357](https://github.com/apple/swift-evolution/blob/main/proposals/0357-regex-string-processing-algorithms.md)
* Summary Author: [Cihat Gündüz](https://fline.dev/about)
* Article: [Swift Evolution Monthly: May ‘22](https://www.fline.dev/swift-evolution-monthly-may-22/#se-0357-regex-powered-string-processing-algorithms)

This one adds a whole bunch of new APIs that will simplify a lot of String processing code but not only that, collection types will profit from these APIs as well! And many of the new APIs are not even Regex-related, so I find the title a bit misleading. For example, there will be a [new `contains` function](https://github.com/apple/swift-evolution/blob/main/proposals/0357-regex-string-processing-algorithms.md?ref=fline.dev#contains) which you can pass a sequence — currently, we can only pass a single element:

```Swift
let chars = ["A", "B", "B", "A"]
chars.contains(["B", "A"])
```

We will also get a [new `ranges` function](https://github.com/apple/swift-evolution/blob/main/proposals/0357-regex-string-processing-algorithms.md?ref=fline.dev#ranges) saving us from writing a loop or reduce function just to get multiple matching ranges, see this example:

```Swift
let searchQuery = "Harry"
let potterQuote = """
	Hagrid: "You're a Wizard, Harry."
	Harry: "I can't be a Wizard. I mean: I'm, just Harry – just Harry!"
	"""
let rangesToHighlight = potterQuore.ranges(of: "Harry")
```

And there are still many APIs we only have access to through `NSString` (and therefore Objective-C), like `[replacingOccurrences(of:with:)](https://developer.apple.com/documentation/foundation/nsstring/1412937-replacingoccurrences?ref=fline.dev)` which will now get a [new `replacing(_:with:)` Swift-native API](https://github.com/apple/swift-evolution/blob/main/proposals/0357-regex-string-processing-algorithms.md?ref=fline.dev#replace).

See [this table](https://github.com/apple/swift-evolution/blob/main/proposals/0357-regex-string-processing-algorithms.md?ref=fline.dev#string-algorithm-additions) for a summarizing list of all new APIs getting added to Swift.

On top of these new APIs, the proposal is also adding a `~=` operator overload for pattern matching with Regexes + a [new `CustomConsumingRegexComponent`protocol](https://github.com/apple/swift-evolution/blob/main/proposals/0357-regex-string-processing-algorithms.md?ref=fline.dev#customconsumingregexcomponent)which will allow Apple & 3rd party library authors to make their own types directly usable in many of the new APIs + within the [Regex builder DSL](https://github.com/apple/swift-evolution/blob/main/proposals/0351-regex-builder.md?ref=fline.dev).
