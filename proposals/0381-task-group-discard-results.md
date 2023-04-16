# DiscardingTaskGroups (Summary)

* Full Proposal: [SE-0381](https://github.com/apple/swift-evolution/blob/main/proposals/0381-task-group-discard-results.md)
* Summary Author: [Cihat Gündüz](https://fline.dev/about)
* Article: [Swift Evolution Monthly: December ‘22](https://www.fline.dev/swift-evolution-monthly-december-22/#se-0381-discardingtaskgroups)

This one is an improvement to Structured Concurrency in Swift, primarily aimed at improving memory management on the server. If you are new to Swifts' new concurrency mechanisms such as `async`/`await` or `with(Throwing)TaskGroup`, you might want to read up on the two most relevant proposals' "Proposed Solution" sections as I can’t summarize them in short here and they are pretty well written: [SE-0296 Async/await](https://github.com/apple/swift-evolution/blob/main/proposals/0296-async-await.md?ref=fline.dev#proposed-solution-asyncawait) and [SE-0304 Structured concurrency](https://github.com/apple/swift-evolution/blob/main/proposals/0304-structured-concurrency.md?ref=fline.dev#proposed-solution).

In short, this proposal introduces two new `TaskGroup` variants (we currently have `TaskGroup` and `ThrowingTaskGroup`) which do not work like a sequence and thus they don’t conform to `AsyncSequence` (unlike `TaskGroup`/`ThrowingTaskGroup`). Instead, they automatically clean up any child `Task`s that complete, and they could  be potentially made to conform to `Sendable`, which is planned for a future proposal. The new types `DiscardingTaskGroup` and `ThrowingDiscardingTaskGroup` are provided as a parameter to the new `with(Throwing)DiscardingTaskGroup` global function. Other than the already mentioned differences, they work exactly like the previous task group types (and their related global function `with(Throwing)TaskGroup`).

The automatic child task clean-up nature makes these new types suitable for any situation where events are consumed for a longer period of time while creating (potentially long-running) tasks. The perfect use case for this is an HTTP or RPC server that handles an indefinite amount of requests. It's not possible to write memory-efficient concurrency code for these kinds of scenarios right now.

It’s impressive to see how Swift on the Server is moving forward steadily over such a long period of time already, and there are still so many more things planned!
