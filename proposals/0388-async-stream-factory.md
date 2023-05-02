# Convenience Async[Throwing]Stream.makeStream methods (Summary)

* Full Proposal: [SE-0388](https://github.com/apple/swift-evolution/blob/main/proposals/0388-async-stream-factory.md)
* Summary Author: [Cihat Gündüz](https://fline.dev/about)
* Article: [Swift Evolution Monthly: March + April ‘23](https://www.fline.dev/swift-evolution-monthly-mar-apr-23/#se-0388-convenience-asyncstreammakestream-methods)

Currently, when creating an `AsyncStream`, you get passed a `continuation` parameter into the trailing closure to send (or `.yield`) values to. In some cases, this works fine. But in many situations, you actually might want to pass around the `continuation` to some other places. And this proposal adds that capability:

```Swift
let (stream, continuation) = AsyncStream.makeStream(of: Int.self)

await withTaskGroup(of: Void.self) { group in
   group.addTask {
      // code using `continuation`
   }

   group.addTask {
      // code using `stream`
   }
}
```

Note the `makeStream(of:)` method returning a tuple containing the `continuation`. It's also worth noting that this is making use of `@backDeployed(before:)` introduced recently in [SE-0376](https://www.fline.dev/swift-evolution-monthly-october-22/#se-0376-function-back-deployment) to make this API available for projects targeting older OS versions than the ones Swift 5.9 will be included in. 💯
