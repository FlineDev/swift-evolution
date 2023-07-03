# Noncopyable structs and enums (Summary)

* Full Proposal: [SE-0390](https://github.com/apple/swift-evolution/blob/main/proposals/0390-noncopyable-structs-and-enums.md)
* Summary Author: [Cihat Gündüz](https://fline.dev/about)
* Article: [Swift Evolution Monthly: May ‘23](https://www.fline.dev/swift-evolution-monthly-may-23/#se-0390-noncopyable-structs-and-enums)


In Swift, we currently have value types (like `struct`) and reference types (like `class`). We use value types to represent blobs of data that are implicitly copied when we pass them along to other functions. We use reference types whenever we don't want this copying behavior and instead want to pass a pointer so there's a single object that can be changed by other functions directly.

This proposal introduces a third concept that can be often used in place of a class in the future: A "noncopyable" type. You can create a struct and conform it to `~Copyable` (read: "suppress Copyable") and when passing it along to functions, it won't be copied implicitly. But unlike classes, there's no overhead of storing the data in the heap (saves memory) or reference counting (saves CPU cycles).

The example provided in the proposal is a `FileDescriptor`:

```swift
struct FileDescriptor: ~Copyable {
  private var fd: Int32

  init(fd: Int32) { self.fd = fd }

  func write(buffer: Data) {
    buffer.withUnsafeBytes { 
      write(fd, $0.baseAddress!, $0.count)
    }
  }

  deinit {
    close(fd)
  }
}
```

Note that noncopyable structs can have a `deinit` method, which normal structs can't have.

There are some requirements for nested types (they need to be `~Copyable`, too) and limitations for generic types (see [workarounds](https://github.com/apple/swift-evolution/blob/main/proposals/0390-noncopyable-structs-and-enums.md?ref=fline.dev#working-around-the-generics-restrictions)). But this sounds like it's going to change some current Best Practices about correct and performant Swift code. Read [this section](https://github.com/apple/swift-evolution/blob/main/proposals/0390-noncopyable-structs-and-enums.md?ref=fline.dev#using-noncopyable-values) to learn more about the usage of noncopyable types.
