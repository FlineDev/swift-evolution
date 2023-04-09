# Clock, Instant, and Duration

* Proposal: [SE-0329](https://github.com/apple/swift-evolution/blob/main/proposals/0329-clock-instant-duration.md)
* Article: [Swift Evolution Monthly: First Issue](https://www.fline.dev/swift-evolution-monthly-first-issue/#se-0329-clock-instant-and-duration)
* Author: [Cihat Gündüz](https://github.com/Jeehut)

**Problem:**
We have many different types to measure & do calculations with continuous time: `Date`, `TimeInterval`, `DispatchTime`, `DispatchTimeInterval`, …Currently, there’s no unified concept of time in all frameworks. Also, `TimeInterval` is technically just a `typealias` for `Double` and is implicitly respresenting time in the unit of “seconds”. There’s no explicitness to the unit, nor are there any APIs available to convert easily between units (like [here](https://github.com/Flinesoft/HandySwift/blob/main/Sources/HandySwift/Extensions/TimeIntervalExt.swift?ref=fline.dev)). Also, as a floating-type, `TimeInterval` is susceptible to [rounding errors](https://docs.oracle.com/cd/E19957-01/806-3568/ncg_goldberg.html?ref=fline.dev#689).

**Solution:**
Three new types will be introduced to unify the concepts of time in Swift:

```Swift
public struct Duration {
  public var components: (seconds: Int64, attoseconds: Int64)
}
```

`Duration` prevents any inexact rounding errors like with `Double` by storing the time interval in two `Int64` variables: `seconds` and `attoseconds`. But this internal representation will probably never be accessed directly, as many convenience extensions are provided, such as a static `.seconds` method to write readable code like `let duration: Duration = .seconds(5)`. `Duration`also supports arithmetic operations such as `+`, `—` or `*` and `/`.

```Swift
public protocol InstantProtocol {
  func advanced(by duration: Duration) -> Self
  func duration(to other: Self) -> Duration
}
```

Types adhering to the `InstantProtocol`, such as `Date` in the future, basically represent an instant in time and have APIs to interact with the `Duration` type. `Date`s `addingTimeInterval()` method gets unified to `advanced(by:)` and `timeIntervalSince()` method gets unified to `duration(to:)`. Types adhering to `InstantProtocol` will also support the arithmetic operations `+`and `-`.

```Swift
public protocol Clock {
  associatedtype Instant: InstantProtocol

  var now: Instant { get }
  func sleep(until deadline: Instant, tolerance: Instant.Duration?) async throws
}

extension Clock {
  func measure(_ work: () async throws -> Void) reasync rethrows -> Instant.Duration
}
```

Lastly, types adhering to the `Clock` protocol, like a new `UTCClock` type in Foundation in the future, will provide a way to get the current time instant via a static `.now` computed property (so `UTCClock.now` will be equal to `Date()`). Also, they will provide an asynchronous `sleep(until:tolerance:)` function. An extra convenience function `measure(work:) -> Duration` will help stopping the time of executing a chunk of code — neat! The `Clock` protocol will also make it easier to create [a custom type](https://github.com/apple/swift-evolution/blob/main/proposals/0329-clock-instant-duration.md?ref=fline.dev#example-custom-clock) for [controlling time in tests](https://github.com/pointfreeco/combine-schedulers?ref=fline.dev#testscheduler).

**Expected in:** **~**Swift 5.7 (my guess based on [merged PR](https://github.com/apple/swift/pull/40609?ref=fline.dev) from Feb 16th)
