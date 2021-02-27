---
title: Get an Array of Dictionary's keys or values
layout: post
categories: Tips
tags: swift
---

If you need an array of keys or values of a dictionary, Swift has an easy way to do it.

```swift
// You can get an array of dictionary's key or values
let grades = ["KOR": 82, "ENG": 98, "MATH": 90]

let subjects = [String](grades.keys)
// ["KOR", "ENG", "MATH"]

let scores = [Int](grades.values)
// [82, 90, 90]
```
