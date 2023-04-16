# Regex Literals (Summary)

* Full Proposal: [SE-0354](https://github.com/apple/swift-evolution/blob/main/proposals/0354-regex-literals.md)
* Summary Author: [Cihat Gündüz](https://fline.dev/about)
* Article: [Swift Evolution Monthly: May ‘22](https://www.fline.dev/swift-evolution-monthly-may-22//#se-0354-regex-literals)

Regex literals will provide a safer way to define regular expressions than passing a pattern `String`: While a pattern `String` can only be checked for syntax errors (such as an opening brace `(` without a closing brace `)`) during runtime (= when the code is executed by the user), the literal can be checked at compile-time (= when you build your app) and therefore it will help reduce crashes for your users or error handling code for you because you don’t have to catch any errors. The syntax will look familiar to those experienced in languages like Ruby, Perl, or JavaScript:

```Swift
let nsRegexFromString = try NSRegularExpression(pattern: "(\\w+)@(.*)")
let regexFromString = try Regex("(\\w+)@(.*)")
let regexFromLiteral = /(\w+)@(.*)/  // <= added in SE-0354
```

Note that in the `/../` literal form, there’s no need to escape characters like `\`to make them work as Regex meta characters. The only character that will require escaping is the `/` itself. To work around this, you’ll be able to surround your pattern `#/.../#` much like you can do today with Strings when you want to use the character `"` inside a String (e.g. `#"Press "Done"."#`). So for a regex matching the WWDC website, you might write something like:

```Swift
let httpUrlRegex = #/http://developer\.apple\.com/wwdc22/#
```

I like that the proposal also covers [typed captures](https://github.com/apple/swift-evolution/blob/main/proposals/0354-regex-literals.md?ref=fline.dev#typed-captures) and [named captures](https://github.com/apple/swift-evolution/blob/main/proposals/0354-regex-literals.md?ref=fline.dev#named-captures) — a nice resource to learn about them with example Swift code that’ll work in the near future. Additionally, some extra love was given to more complex multi-line literals. They will support comments via `#` and ignore whitespaces for alignment of parts of regexes for better readability, see this example:

```Swift
let regex = #/
	# Match e.g. "DEBIT	Totally Legit Shell Corp	$2,000,000.00"
	(?<kind>	\w+)				\s\s+
	(?<account>	(?: (?!\s\s) . )+)	\s\s+	# may contain spaces
	(?<amount> 	.*)
	/#
```

The (?<name> pattern) is a “named capture” that can be referenced by `.name` rather than`.0`.

Because `/` is used in other places in Swift today, the introduction of Regex literals is a [source-breaking change](https://github.com/apple/swift-evolution/blob/main/proposals/0354-regex-literals.md?ref=fline.dev#source-compatibility) and will be disabled by default until Swift 6 is released. You’ll be able to activate them in your project by passing `-enable-bare-regex-syntax` in Swift 5.x mode, or just use `#/.../#`.
