---
title: How to Join an Array of Strings
layout: post
categories: Tips
tags: swift array
---

Using `joined()` method, we can merge an array of strings to a single string. We can add a separator too.

```swift
let names = ["Brian", "Nick", "John", "David"]
let list = names.joined(separator: ", ")
print(list)
// "Brian, Nick, John, David"
```