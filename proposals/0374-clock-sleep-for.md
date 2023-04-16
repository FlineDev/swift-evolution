# Add sleep(for:) to Clock (Summary)

* Full Proposal: [SE-0374](https://github.com/apple/swift-evolution/blob/main/proposals/0374-clock-sleep-for.md)
* Summary Author: [Cihat Gündüz](https://fline.dev/about)
* Article: [Swift Evolution Monthly: October ‘22](https://www.fline.dev/swift-evolution-monthly-october-22/#se-0374-add-sleepfor-to-clock)

When introducing the new `Clock`, `Instant` and `Duration` types with [SE-0329](https://github.com/apple/swift-evolution/blob/main/proposals/0329-clock-instant-duration.md?ref=fline.dev) (see [my summary](https://www.fline.dev/swift-evolution-monthly-first-issue/#se-0329-clock-instant-and-duration)), it seems that one important method on the `Clock` type was forgotten: A `sleep(for: Duration)` method. This caused a problem when [Brandon Williams](https://twitter.com/mbrandonw?ref=fline.dev) and [Stephen Celis](https://twitter.com/stephencelis?ref=fline.dev) from [Point-Free](https://www.pointfree.co/?ref=fline.dev) used these new APIs to write tests and control time in them for fast unit testing. The reason for their problem was that the existing method `sleep(until: Instant)` didn't allow for using an existential clock type to switch out the real-time `Clock` in tests with a custom fast-forwardable `Clock` due to the type-erased `Instant` needed for the `sleep(until: Instant)` method.

With this proposal they added a new `sleep(for: Duration)` method that has no such problem. I'm glad that the community has such great members who play around with new APIs even before they are released so they can already propose a fix to some rough edges before most of us even had a chance to try them out.

Not only have they fixed the Swift API for us, but they also [open-sourced a new library](https://github.com/pointfreeco/swift-clocks?ref=fline.dev) which comes with 3 `Clock` types included: `TestClock` for unit tests, `ImmediateClock` for SwiftUI previews and `UnimplementedClock` for mocking!
