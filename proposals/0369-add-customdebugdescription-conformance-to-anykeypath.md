# Add CustomDebugStringConvertible conformance to AnyKeyPath (Summary)

* Full Proposal: [SE-0369](https://github.com/apple/swift-evolution/blob/main/proposals/0369-add-customdebugdescription-conformance-to-anykeypath.md)
* Summary Author: [Cihat Gündüz](https://fline.dev/about)
* Article: [Swift Evolution Monthly: September ‘22](https://www.fline.dev/swift-evolution-monthly-september-22/#se-0369-add-customdebugstringconvertible-conformance-to-anykeypath)

This proposal aims at improving the `print`ing of key paths such as in `print(\User.name)` from currently resulting in `Swift.KeyPath<User, String>` which is not very useful, to result in something closer to the call like `\User.name`.

This small improvement could help in debugging and make unit tests clearer.
