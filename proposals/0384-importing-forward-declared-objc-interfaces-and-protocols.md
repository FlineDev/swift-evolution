# Importing Forward Declared Objective-C Interfaces and Protocols (Summary)

* Full Proposal: [SE-0384](https://github.com/apple/swift-evolution/blob/main/proposals/0384-importing-forward-declared-objc-interfaces-and-protocols.md)
* Summary Author: [Cihat Gündüz](https://fline.dev/about)
* Article: [Swift Evolution Monthly: Jan + Feb ‘22](https://www.fline.dev/swift-evolution-monthly-jan-feb-22/#se-0384-importing-forward-declared-objective-c-interfaces-and-protocols)

This concerns everyone calling code written in Objective-C from within Swift (like when using older libraries). The idea is to port a common workaround for cyclic dependencies in Objective-C code to Swift in an automatically synthesized way:

In Objective-C, the public API of a type is declared in a header `.h` file. When a type named `User` wants to declare a public property `credentials` which has its own type `Credentials`, it needs to import its header file  `Credentials.h`. Now, when the `Credentials` type had a property named `user` of type `User`, it would need to import the header file `User.h` which is a cycling dependency that is not allowed and causes a compiler error. To fix this, there's a workaround in Objective-C where one declares an empty type to specify that a specific type "exists" without actually importing its header, this technique is called "[forward-declaration](https://en.wikipedia.org/wiki/Forward_declaration?ref=fline.dev)".

To use such Objective-C APIs in Swift, one had to "forward-declare" those types in Swift explicitly, which is very inconvenient. In the future, that won't be necessary. Swift will automatically synthesize forward-declared types in Swift as follows:

```Swift
// @class Foo turns into
@available(*, unavailable, message: “This Objective-C class has only been forward declared; import its owning module to use it”)
class Foo : NSObject {}

// @protocol Bar turns into
@available(*, unavailable, message: “This Objective-C protocol has only been forward declared; import its owning module to use it”)
protocol Bar : NSObjectProtocol {}
```

This new behavior will be turned on by default in Swift 6, but needs to be opted-in with Swift 5.x by using the flag `-enable-import-objc-forward-declarations`.
