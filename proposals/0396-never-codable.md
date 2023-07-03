# Conform `Never` to `Codable` (Summary)
 
* Full Proposal: [SE-0396](https://github.com/apple/swift-evolution/blob/main/proposals/0396-never-codable.md)
* Summary Author: [Cihat Gündüz](https://fline.dev/about)
* Article: [Swift Evolution Monthly: May ‘23](https://www.fline.dev/swift-evolution-monthly-may-23/#se-0396-conform-never-to-codable)

This is probably the shortest proposal I've ever summarized, so let's keep it short: The proposal suggests adding `Codable` conformance to the `Never` type in Swift. This allows encoding but throws an error for decoding attempts, addressing a limitation with generic types that involve `Never`, like `Either<Int, Never>`.
