# Allow implicit `self` for `weak self` captures, after `self` is unwrapped (Summary)

* Full Proposal: [SE-0365](https://github.com/apple/swift-evolution/blob/main/proposals/0365-implicit-self-weak-capture.md)
* Summary Author: [Cihat Gündüz](https://fline.dev/about)
* Article: [Swift Evolution Monthly: July ‘22](https://www.fline.dev/swift-evolution-monthly-july-22/#se-0365-allow-implicit-self-for-weak-self-captures-after-self-is-unwrapped)

Since Swift 5.3 it is possible to write a closure and capture `[self]` in order to not have to write `self.` in front of every call on properties and functions of the current type within the body of the closure (thanks to [SE-0269](https://github.com/apple/swift-evolution/blob/main/proposals/0269-implicit-self-explicit-capture.md?ref=fline.dev)) like this:

```Swift
button.tapHandler = { [self] in
   dismiss()  // no need to write `self.dismiss()`
}
```

This proposal aims to introduce this same behavior for `[weak self]` as well when and only after the `Optional` of `self` is unwrapped. So for example a `guard let self = self else { return }` (or the [newly possible](https://www.fline.dev/swift-evolution-monthly-first-issue/#se-0345-if-let-shorthand-for-shadowing-an-existing-optional-variable) `guard let self { return }`) would make `self.` calls for properties and functions unnecessary:

```Swift
button.tapHandler = { [weak self] in
   guard let self { return }
   dismiss()  // no need to write `self.dismiss()`
}
```
