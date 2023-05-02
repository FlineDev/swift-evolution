# Package Registry Publish (Summary)

* Full Proposal: [SE-0391](https://github.com/apple/swift-evolution/blob/main/proposals/0391-package-registry-publish.md)
* Summary Author: [Cihat Gündüz](https://fline.dev/about)
* Article: [Swift Evolution Monthly: March + April ‘23](https://www.fline.dev/swift-evolution-monthly-mar-apr-23/#se-0391-package-registry-publish)

This proposal defines all things missing to allow for a unified way of publishing Swift packages to registries like the now [officially](https://www.swift.org/blog/swift-package-index-developer-spotlight/?ref=fline.dev) Apple-backed [Package Index](https://swiftpackageindex.com/?ref=fline.dev):

- A metadata format for related URLs, authors & organizations.
- A package signature format for registries that require signing.
- A new `package-registry publish` subcommand to "create a package source archive, sign it if needed, and publish it to a registry" all in one go.
- A set of requirements for registries supporting signing to ensure security.

The good news is that you probably won't have to learn more about the details of this proposal thanks to the great work of [Dave](https://github.com/daveverwer?ref=fline.dev) & [Sven](https://github.com/finestructure?ref=fline.dev), plus the support of the Swift community, including [these 85 amazing people](https://swiftpackageindex.com/supporters?ref=fline.dev). All that framework authors might need is the new `publish` subcommand, maybe not even that unless they use a CI for uploads, cause Apple might ship a UI for it in Xcode. 🤞
