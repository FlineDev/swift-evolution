# `if let` shorthand for shadowing an existing optional variable (Summary)

* Proposal: [SE-0345](https://github.com/apple/swift-evolution/blob/main/proposals/0345-if-let-shorthand.md)
* Summary Author: [Cihat Gündüz](https://github.com/Jeehut)
* Article: [Swift Evolution Monthly: First Issue](https://www.fline.dev/swift-evolution-monthly-first-issue/#se-0345-if-let-shorthand-for-shadowing-an-existing-optional-variable)

**Problem:**

Optional binding as in `if let foo = foo { … }` is extremely common in Swift code. Repeating the same identifier twice is verbose and can lead to weird indentation situations when line limits are reached with long variable names. Solving this verbosity was already [requested back in 2015](https://forums.swift.org/t/if-let-shortcut-syntax/56?ref=fline.dev) by the community, shortly after Swift had been open sourced.

**Solution:**

Instead of repeating the variable name like in this code:

```Swift
let someLengthyVariableName: Foo? = ...
let anotherImportantVariable: Bar? = ...

if let someLengthyVariableName = someLengthyVariableName, let anotherImportantVariable = anotherImportantVariable {
    ...
}
```

We would be able to get rid of the right `= repeatingVariableName` part like so:

```Swift
let someLengthyVariableName: Foo? = ...
let anotherImportantVariable: Bar? = ...

if let someLengthyVariableName, let anotherImportantVariable {
    ...
}
```

**Expected in: **~Swift 5.7 ([small PR](https://github.com/apple/swift/pull/40694/files?ref=fline.dev) 👍, but [Review feedback is quite mixed](https://forums.swift.org/t/se-0345-if-let-shorthand-for-shadowing-an-existing-optional-variable/55805?ref=fline.dev))