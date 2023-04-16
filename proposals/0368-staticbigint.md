# StaticBigInt (Summary)

* Full Proposal: [SE-0368](https://github.com/apple/swift-evolution/blob/main/proposals/0368-staticbigint.md)
* Summary Author: [Cihat Gündüz](https://fline.dev/about)
* Article: [Swift Evolution Monthly: September ‘22](https://www.fline.dev/swift-evolution-monthly-september-22/#se-0368-staticbigint)

Unless you're working on a mathematical app or framework, you probably won't need to know about the new `StaticBigInt` type introduced here. But if you do, you might want to read the full proposal (it's short enough) to learn how you can define your own large integer types that support literals that don't fit into `[U]Int64`.

Maybe also worth noting that the official [swift-numerics](https://github.com/apple/swift-numerics?ref=fline.dev) library might soon get [larger integer types](https://github.com/apple/swift-numerics/issues/4?ref=fline.dev) such as `[U]Int256` once this was accepted.
