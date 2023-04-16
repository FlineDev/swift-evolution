# Unaligned Loads and Stores from Raw Memory (Summary)

* Full Proposal: [SE-0349](https://github.com/apple/swift-evolution/blob/main/proposals/0349-unaligned-loads-and-stores.md)
* Summary Author: [Cihat Gündüz](https://fline.dev/about)
* Article: [Swift Evolution Monthly: April ‘22](https://www.fline.dev/swift-evolution-monthly-april-22/#se-0349-unaligned-loads-and-stores-from-raw-memory)

Helps prevent alignment mismatches in low-level byte data parsing by introducing a new loadUnaligned function.
[Probably](https://github.com/apple/swift/pull/41033?ref=fline.dev) ships with Swift 5.7.
