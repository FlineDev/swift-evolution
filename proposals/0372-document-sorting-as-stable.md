# Document Sorting as Stable (Summary)

* Full Proposal: [SE-0372](https://github.com/apple/swift-evolution/blob/main/proposals/0372-document-sorting-as-stable.md)
* Summary Author: [Cihat Gündüz](https://fline.dev/about)
* Article: [Swift Evolution Monthly: September ‘22](https://www.fline.dev/swift-evolution-monthly-september-22/#se-0372-document-sorting-as-stable)

Imagine you have an array of books in code, something like this:

```Swift
var books: [Book] = [
   .init(author: "Robert Galbraith", title: "The Cuckoo's Calling"),
   .init(author: "J. R. R. Tolkien", title: "The Fellowship of the Ring"),
   .init(author: "J. R. R. Tolkien", title: "The Two Towers"),
   .init(author: "J. R. R. Tolkien", title: "The Return of the King"),
]
```

And you call `books.sort(by: { $0.author < $1.author })`, you would expect that the three Lord of the Rings books from J. R. R. Tolkien would be sorted above the first Cormoran Strike book by J. K. Rowling, right? You're right, of course.

But would you also expect the three Lord of the Rings books to remain in the right order, starting with "The Fellowship of the Ring"? I guess you would. It's how humans would naturally sort. But technically speaking, all three Lord of the Rings books have the same author, and therefore the sorting criteria treats them all like they're on the same level. How elements that are on the same level are treated depends on the sorting algorithm. Algorithms that *preserve* the order of equal-level elements are called "stable".

In Swift, the `sort` function has been implemented as a "stable" algorithm for multiple years now, but it was never documented to be. So in theory, the implementation of future versions could have changed to a non-stable algorithm. Not anymore: This proposal documents the `sort` function's behavior to be stable.
