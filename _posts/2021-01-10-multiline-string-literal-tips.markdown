---
title: Multiline string literal tips
layout: post
categories: Tips
tags: swift strings
---

In Swift, we can use multiline string literal to express several lines of string. Although, adding the new line character `\n` can create line break, it only works for string that are displayed. Use multitline strings for formatting string nicely.

```swift
let quotation = """
We can make multi-line strings.
Use three double quotation marks.
Also, multi-line strings can write "quote marks"
"""
```

<br>
When using multiline string, always start and end the content using `"""` on single line.

```swift
let badMultilineString = """This will not create multi-line string.
Always start with an new line and end before a new line."""

let goodMultilineString = """
This will create multi-line string.
Always start with an new line and end before a new line.
"""
```

<br>
We can also add backslash `\` for reading a very long string a bit easier. This will not change the value.

```swift
let veryLongString = """
Lorem Ipsum is simply dummy text of the printing and typesetting industry.
Lorem Ipsum has been the industry's standard dummy text ever \
since the 1500s, when an unknown printer took a galley of \
type and scrambled it to make a type specimen book.
"""
```

<br>
We can indent strings in multiline string. The indent will start at the closing quotation marks `"""`, before will be ignored. So in the following code, the second string will have indents.

```swift
let linesWithIndent = """
    This line doesn't have indents.
        This line has indents.
    This line also doesn't have indents.
    """
```

<br>
When building a longer strings with multiline string literals, every line in the string needs to end with a line break.

```swift
let badStart = """
one
two
"""
let end = """
three
"""
print(badStart + end)
// Prints two lines:
// one
// twothree

let goodStart = """
one
two

"""
print(goodStart + end)
// Prints three lines:
// one
// two
// three
```