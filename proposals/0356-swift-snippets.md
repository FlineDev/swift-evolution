# Swift Snippets (Summary)

* Full Proposal: [SE-0356](https://github.com/apple/swift-evolution/blob/main/proposals/0356-swift-snippets.md)
* Summary Author: [Cihat Gündüz](https://fline.dev/about)
* Article: [Swift Evolution Monthly: May ‘22](https://www.fline.dev/swift-evolution-monthly-may-22/#se-0356-swift-snippets)

This proposal at first might sound like the [Xcode snippets](https://nshipster.com/xcode-snippets/?ref=fline.dev) feature, I personally even thought (or hoped?) it was related to [Swift scripts](https://forums.swift.org/t/pitch-swiftpm-support-for-swift-scripts-revision/46717?ref=fline.dev) in some way. But none of that is true. This proposal is mostly aimed at library authors and provides them with another way to document functionality. This sentence in the proposal summarizes the direction of the feature pretty well, I think:

> Making it easy to add code listings to documentation that can also be built and run (and validated) is one of the main goals of snippets.

While [sample projects](https://github.com/pointfreeco/swift-composable-architecture/tree/main/Examples?ref=fline.dev) are great to show off how everything fits together, they are hard to maintain and don’t help with exploration to potential users. [Extensive code examples](https://github.com/pointfreeco/swift-composable-architecture/blob/main/Sources/ComposableArchitecture/ViewStore.swift?ref=fline.dev#L4-L53) right within documentation comments are much more explorable because you can easily show them via alt-clicking APIs right within Xcode when you need them. But from the compiler’s view, they are just plain Strings, and thus when you make changes to your APIs, it’s easy to forget to update the related code examples accordingly and they get outdated.

Snippets fix the latter. You provide them as separate files in a `Snippets` folder within your Swift package & you can also group them, like so:

![https://miro.medium.com/max/544/1*gv1CSs1yncJRGnzZz4dOpg.png](https://miro.medium.com/max/544/1*gv1CSs1yncJRGnzZz4dOpg.png)

Then, in your documentation comments, instead of placing sample code via three backticks, you’ll be able to just use the new DocC directive `@Snippet`:

```Swift
@Snippet(path: "my-package/Snippets/Snippet1")
```

This will place the *presentation code* of your snippet file right into the documentation view within Xcode or add it to your HTML file. What is the *presentation code*, you ask? Well, because all snippet files will be Swift files that we will be able to compile via `swift build --build-snippets` (and library authors should make sure to run that on their CI), you might need to write some helper code that is just churn and might not be relevant to showing off your functionality. The proposal gives us a way to hide some parts of our code, for that we simply add `// MARK: HIDE` and `// MARK: SHOW`:

```Swift
//! Line comments prefixed with ! will
//! serve as the snippet's short description.

import ComposableArchitecture
import SwiftUI

// MARK: HIDE
struct SampleView: View {
	let store: Store<State, Action>

	// MARK: SHOW
	var body: some View {
		WithViewStore(self.store) { viewStore in
			VStack {
				Text("Current count: \(viewStore.count)")
				Button("Increment") {
					viewStore.send(.incrementButtonTapped)
				}
			}
		}
	}
	// MARK: HIDE
}
```

The highlighted code is the “*presentation code”* in this example. Note also the description at the top.

The cool thing is that all snippet files will get full access to all targets of your package, you just need to import them. External dependencies can’t be imported as part of this proposal, but they are mentioned in [future directions](https://github.com/apple/swift-evolution/blob/main/proposals/0356-swift-snippets.md?ref=fline.dev#future-directions).

Lastly, the proposal also mentions that snippets could be provided as standalone repositories, without any (real) package attached:

> Examples of snippet-only packages might be a collection of recipes for composing UI elements in new and interesting ways, teaching the Swift language snippet by snippet, or providing exercises for a textbook.

I can already see some great community projects on the horizon here … 🌅
