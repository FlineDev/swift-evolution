# `borrowing` and `consuming` parameter ownership modifiers (Summary)

* Full Proposal: [SE-0377](https://github.com/apple/swift-evolution/blob/main/proposals/0377-parameter-ownership-modifiers.md)
* Summary Author: [Cihat Gündüz](https://fline.dev/about)
* Article: [Swift Evolution Monthly: October ‘22](https://www.fline.dev/swift-evolution-monthly-october-22/#se-0377-borrow-and-take-parameter-ownership-modifiers)

This proposal gives Swift authors who are highly concerned about the performance of their code fine-grained control over some aspects of [Automatic Reference Counting](https://en.wikipedia.org/wiki/Automatic_Reference_Counting?ref=fline.dev), the memory management feature of Swift. Two new parameter modifiers (like `inout`) are introduced for that, namely `borrow` and `take`.

The vast majority of app developers won't need to use these modifiers and should instead trust Swift's built-in memory management heuristics. For those who are interested, please [read the full proposal](https://github.com/apple/swift-evolution/blob/main/proposals/0377-parameter-ownership-modifiers.md?ref=fline.dev#introduction) to fully understand the new modifiers.
