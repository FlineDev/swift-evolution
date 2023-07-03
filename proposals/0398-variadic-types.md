# Allow Generic Types to Abstract Over Packs (Summary)

* Full Proposal: [SE-0398](https://github.com/apple/swift-evolution/blob/main/proposals/0398-variadic-types.md)
* Summary Author: [Cihat Gündüz](https://fline.dev/about)
* Article: [Swift Evolution Monthly: May ‘23](https://www.fline.dev/swift-evolution-monthly-may-23/#se-0398-allow-generic-types-to-abstract-over-packs)

This introduces the ability to use generic types to abstract over packs of types. It generalizes the concepts introduced in [SE-0393](https://www.fline.dev/swift-evolution-monthly-mar-apr-23/#se-0393-value-and-type-parameter-packs), which allowed generic function declarations to abstract over a variable number of types (`each T`). With this proposal, you can define generic type declarations that abstract over *multiple* types, enabling the creation of more flexible and generalized algorithms.

This allows defining functions like `zip` where the return type needs an arbitrary number of type parameters – one for each input sequence:

```Swift
func zip<each S>(_seq: repeat each S) -> ZipSequence<repeat each S>
  where repeat each S: Sequence
```

The return type `ZipSequence<repeat each S>` is what this proposal allows.
