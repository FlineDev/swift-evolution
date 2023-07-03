# Custom Actor Executors (Summary)

* Full Proposal: [SE-0392](https://github.com/apple/swift-evolution/blob/main/proposals/0392-custom-actor-executors.md)
* Summary Author: [Cihat Gündüz](https://fline.dev/about)
* Article: [Swift Evolution Monthly: May ‘23](https://www.fline.dev/swift-evolution-monthly-may-23/#se-0392-custom-actor-executors)

This proposal introduces the concept of custom executors and customization points in Swift Concurrency, providing developers with greater control over the execution semantics of their actors. I'm not in a position to customize executors, so I can't provide practical implications. But the motivation section ends with this:

> Along with introducing ways to customize where code executes, this proposal also introduces ways to assert and assume the appropriate executor is used. This allows for more confidence when migrating away from other concurrency models to Swift Concurrency.
