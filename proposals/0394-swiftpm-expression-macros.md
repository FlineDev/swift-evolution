# Package Manager Support for Custom Macros (Summary)

* Full Proposal: [SE-0394](https://github.com/apple/swift-evolution/blob/main/proposals/0394-swiftpm-expression-macros.md)
* Summary Author: [Cihat Gündüz](https://fline.dev/about)
* Article: [Swift Evolution Monthly: March + April ‘23](https://www.fline.dev/swift-evolution-monthly-mar-apr-23/#se-0394-package-manager-support-for-custom-macros)

With [SE-0382](https://www.fline.dev/swift-evolution-monthly-jan-feb-23/#se-0382-expression-macros) "Expression Macros" & [SE-0389](https://www.fline.dev/swift-evolution-monthly-mar-apr-23/#se-0389-attached-macros) "Attached Macros", we're getting closer to a Swift world full of macros. But what if we wanted to share macros across projects or with the community in an open-source package?

This proposal adds a new target type to the Swift package manifest named `macro` which has the same shape as other target types like `target` or `testTarget`. But it behaves more like the `plugin` target type added in [SE-0303](https://github.com/apple/swift-evolution/blob/main/proposals/0303-swiftpm-extensible-build-tools.md?ref=fline.dev): When included in a project, the `macro` targets are built as executables so that the compiler can run them as part of the compilation process for any targets which depend on them. Also, like package plugins, the macro target will be executed in a sandbox that prevents file system and network access.

I'm looking forward to the macros the community will come up with, especially if Apple introduces how to use these new features to a wider audience at WWDC.
