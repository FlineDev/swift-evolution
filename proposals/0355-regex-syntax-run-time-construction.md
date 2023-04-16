# Regex Syntax and Run-time Construction (Summary)

* Full Proposal: [SE-0355](https://github.com/apple/swift-evolution/blob/main/proposals/0355-regex-syntax-run-time-construction.md)
* Summary Author: [Cihat Gündüz](https://fline.dev/about)
* Article: [Swift Evolution Monthly: May ‘22](https://www.fline.dev/swift-evolution-monthly-may-22/#se-0355-regex-syntax-and-run-time-construction)

This proposal goes into all the details of how the String pattern in the throwing `Regex.init(String)` initializer already shown above (where only runtime checks are possible) will be processed. I think it’s a great reference for everybody running into problems with more complex Regexes long term.

For everyone else, the main takeaway is that the Swift engine will be superior to any other Regex engine because it will support a [wide range](https://github.com/apple/swift-evolution/blob/main/proposals/0355-regex-syntax-run-time-construction.md?ref=fline.dev#syntax) of existing engines/standards and character classes. So it’ll be highly likely that copy & pasted patterns written for other platforms will work in Swift!
