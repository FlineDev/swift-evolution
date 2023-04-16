# Regex builder DSL (Summary)

* Full Proposal: [SE-0351](https://github.com/apple/swift-evolution/blob/main/proposals/0351-regex-builder.md)
* Summary Author: [Cihat Gündüz](https://fline.dev/about)
* Article: [Swift Evolution Monthly: April ‘22](https://www.fline.dev/swift-evolution-monthly-april-22/#se-0351-regex-builder-dsl)

This adds a SwiftUI-like DSL for constructing a `Regex`. The email Regex with two capture groups from above would look something like this:

```Swift
// equivalent to pattern "(\w+)@(.*)"
let emailRegex = Regex {
	Capture {
		OneOrMore(.word)
	}
	"@"
	Capture {
		ZeroOrMore(.any)
	}
} // => Regex<(Substring, Substring, Substring)>
```

Furthermore, a `ChoiceOf` builder would allow specifying multiple variants (replaces `A|B`) and even a `TryCapture {} transform: {}` would be available to directly transform a sub-match into an expected type, such as into an `enum`. `Optionally` (for `A?`) and `Repeat` (for `A{n,m}`) complete the DSL:

```Swift
enum TLD: String {
	case commercial = "com"
	case organization = "org"
	case education = "edu"
}

// same as "https?/{2}\w+(com|org|edu)"
let urlRegex = Regex {
	"http"
	Optionally("s")
	Repeat("/", count: 2)
	OneOrMore(.word)
	TryCapture {
		ChoiceOf {
			"com"
			"org"
			"edu"
		}
	} transform: {
		TLD(rawValue: String($0))
	}
} // => Regex<(Substring, TLD)>
```

Note that the output tuple holds a strongly typed `TLD` instance.

This builder DSL doesn’t only make Regexes much more readable and easier to reason about, it also allows for extracting portions of the code out into their own smaller Regexes, much like we can extract out parts of the `body` in SwiftUI views. The new DSL should remove the hurdle for those who were put off by Regexes due to the cryptic syntax. And I can easily imagine the community creating libraries of common reusable Regexes, such as for matching email addresses.
