# `AsyncStream` and `AsyncThrowingStream` (Summary)

* Full Proposal: [SE-0314](https://github.com/apple/swift-evolution/blob/main/proposals/0314-async-stream.md)
* Summary Author: [Cihat Gündüz](https://fline.dev/about)
* Article: [Swift Evolution Monthly: March + April ‘23](https://www.fline.dev/swift-evolution-monthly-mar-apr-23/#accepted-proposals)

We all have used many types conforming to the `Sequence` protocol, like `Array`, `Dictionary`, or `String`. It allows for easy iteration with  `for element in array`. The same treatment was added to the new Swift Concurrency world with `AsyncSequence`, which is pretty much the same as `Sequence` but the values aren't immediately available, instead they come at a later time. But usage looks exactly the same, with only the `await` keyword added: `for await element in array`.

Actually, there's one thing wrong with the sample code just provided: `array`. Because the `Array` type doesn't conform to `AsyncSequence`, instead, we need new types that we can instantiate and return in our APIs, or that we can consume from system APIs. And that's where `AsyncStream` and its throwing counterpart `AsyncThrowingStream` come into play. If you're not familiar with streams, think of them as "async arrays". This is a simplification of course, and creating an `AsyncStream` is quite different from an array. Here's a simple example:

```Swift
let swiftFileURL: URL = ...
let matches: (String) -> Bool = { $0.contains("// TODO:") }

let matchingLineNumbersStream = AsyncStream<Int> { continuation in
   Task {
      var lineNumber: Int = 0
      for try await line in swiftFileUrl.lines {
         lineNumber += 1
         if matches(line) {
            continuation.yield(lineNumber)
         }
      }

      continuation.finish()
   }
}
```

Note that we're being passed a `continuation` which we pass new values to by calling `yield()` and when we reach the end of the "async array", we call `.finish()`. This way we don't have to write a custom iterator logic.

Then, consumers like some rule in a linting tool could use the stream like so:

```Swift
for await lineNumber in matchingLineNumbersStream {
   violatingLines.append(lineNumber)
}
```

The cool thing about `AsyncStream` is that it's not only useful for slow APIs, like calling data from a server. But it can also be used when receiving values very fast, faster actually than we can consume them on the call site. `AsyncStream` automatically buffers values in this case and sends them in the receiving order, so we never skip any values and things work as expected.
