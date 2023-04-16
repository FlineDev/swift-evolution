# Constrained Existential Types (Summary)

* Full Proposal: [SE-0353](https://github.com/apple/swift-evolution/blob/main/proposals/0353-constrained-existential-types.md)
* Summary Author: [Cihat Gündüz](https://fline.dev/about)
* Article: [Swift Evolution Monthly: April ‘22](https://www.fline.dev/swift-evolution-monthly-april-22/#se-0353-constrained-existential-types)

In the [previous issue](https://jeehut.medium.com/swift-evolution-monthly-first-issue-7d276705c4e0?sk=ad1693bf56e189748f478e3538844aef&ref=fline.dev), we discussed [SE-0346](https://github.com/apple/swift-evolution/blob/main/proposals/0346-light-weight-same-type-syntax.md?ref=fline.dev) which allows us to specify generic types in place instead of specifying them with a `where` clause at the end. This proposal aims to bring the same convenience to existential types with the `any`keyword:

```Swift
protocol P<A, B, C> {}

// the following will be equivalent
var valuesNow: [any P] where P.A == X, P.B == Y, P.C == Z
var valuesNew: [any P<X, Y, Z>]
```
