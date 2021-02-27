---
title: WWDC20 Dub Dub Diary, Part 4 - Swift type inference, writing tests to fail
layout: post
categories: WWDC
---

For the fourth day of WWDC20, among great sessions there were two most interesting sessions about Swift language and testing. This article will talk about these two amazing sessions.

## Embrace Swift type inference

### What is type inference?

In Swift, constant and variable needs to be specified a type at compile time. But, the compiler can figure out the type. This is able because Swift supports type inference. We can write clean, concise code with the help of Swift using type inference.

```swift
// Three types of declaration
let x = ""    // compiler can able to figure out the type
let x: String = ""
let x = "" as String

```

This session explains how the compiler figures out the type with our code, like solving the puzzle. 

### Leverage type inference at the call-site

To explain how the compiler figures out, below function was showed as an example. It was used on a SwiftUI app, to filter data.

```swift
public struct FilteredList<Element, FilterKey, RowContent> {
    public init(_ data: [Element],
                filterBy key: KeyPath<Element, FilterKey>,
                isIncluded: @escaping (FilterKey) -> Bool,
                @ViewBuilder rowContent: @escaping (Element) -> RowContent)
}
```

```swift
FilteredList(
    smoothies,
    filterBy: \.title,
    isIncluded: { title in title.hasSubstring(searchPhrase) }
) { smoothie in
    SmoothieRowView(smoothie: smoothie)
}
```

Type inference helps us write source code faster, not spelling all the types in our code. Below is the code side by side with the initializer.

![Swift Type Inference](/assets/img/2020/08/10/image1.png)

Below code is how the compiler use type inference, using clues from the source code.

```swift
FilteredList<Smoothie, String, SmoothieRowView>(
    smoothies as [Smoothie],
    filterBy: \Smoothie.title as KeyPath<Smoothie, String>,
    isIncluded: { (title: String) -> Bool in title.hasSubstring(searchPhrase) }
) { (smoothie: Smoothie) -> SmoothieRowView in
    SmoothieRowView(smoothie: smoothie)
}
```

I just showed small parts from the session, and I recommend seeing the session. It also covers how the compiler errors, and how to track and resolve it. For in-depth, learn more about Swift Generics. After watching this session, I wanted to try to create something like the example. Wish I could make it and write an article in-depth of type inference.

[Embrace Swift type inference - WWDC 2020 - Videos - Apple Developer](https://developer.apple.com/videos/play/wwdc2020/10165/)

<br>

## Write Tests to Fail

Besides of the session title, this session was great demonstrating a walkthrough to how to make tests for the app. Currently, I've been trying to write a test code for our app, so this session really help me from scratch.

While getting the tests to green is important, writing the test to fail is also great for catching bugs. This is why we need to write tests to fail.

### Set Up

Use `func setUpWithError() throws` to take advantage of error management. And using `launchArguments` or `launchEnvironment` to set state of the app.

```swift
class RecipesTests: XCTestCase {
    let app = FrutaApp()

    override func setUpWithError() throws {
        continueAfterFailure = false
        app.launchArguments.appen("-recepies-tests")
        app.launch()
}
```

- Use `func setUpWithError() throws`
- Perform common class setup tasks
- Leverage `launchArguments` or `launchEnvironment` to set state
- Adopt product changes to focus testing

### Test: actions

Using enum cases for all String value is convenient for future updates. String values can change, so capturing it with enum will be easy to update and reduce misspelling strings. Factor common code into helper functions is good for reducing repetitive codes.

- Design tests for a specific goal
- Use enums
- Factor common code into helper functions
- Model UI hierarchy in testing code
- Consider using a framework or Swift package to share testing code

### Test: assertions

For assertion messages, make use of optional descriptions to give more context for failure messages. And for testing asynchronous logic, use `waitForExistence()` then `sleep()`.

![Use assertion message](/assets/img/2020/08/10/image2.png)

To unwrap optionals, don't force unwrap to cause crash. Instead, follow below code.

```swift
if let favs = favorites { }
guard let favs = favorites else { /* throw an error */ }
let favs = favorites ?? []
let favs = try XCTUnwrap(favorites, "favorites is nil, so there is nothing to count")
```

- Add assertion messages
- Use relevant `XCTAssert*` function
- Unwrap optionals
- Use `waitForExistence()` for asynchronous events
- Throw errors from shared code
- Use `XCTContext.runActivity()` and attachments
- Consider `XCTSkip()`

### Tear down

Use `func tearDownWithError() throws` to take advantage of the new error management.

- Use `func tearDownWithError() throws`
- Collect additional logging
- Reset the environment

[Write tests to fail - WWDC 2020 - Videos - Apple Developer](https://developer.apple.com/videos/play/wwdc2020/10091/)

## Wrap up

Watching these two sessions gave me a refresh of Swift Language and Testing, away from SwiftUI spotlight. I recommend these sessions, it's not very complicated subjects, and really made my mind blown.
