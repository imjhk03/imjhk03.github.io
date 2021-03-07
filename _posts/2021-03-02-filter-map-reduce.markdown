---
title: 'Higher Order Functions: Filter, Map, Reduce'
layout: post
author: Joohee Kim
tags: swift
---

There are some times we need to iterate an array or dictionary to collect or manipulate values. The easy way is using for-in loop, get a value and add or manipulate it to a new array. But Swift offers a great functions that makes it a little cleaner. These functions are called "Higer Order Functions", and they are `filter`, `map`, `reduce` and more.

It takes some time to get used to it. I personally try to use these functions, but doesn't pop it right away while coding. So, I'm hopefully to get used to it writing this post. (Or comeback to remind it again 😅)

# Filter

The name of the function explains it, it returns an array containing the elements that satisfy the condition in order.

```swift
// Filter
let familyNames = ["Park", "Lee", "Jong", "Kim", "Choi"]
let longFamilyNames = familyNames.filter { $0.count > 3 }
print(longFamilyNames)
// ["Park", "Jong", "Choi"]


// For-in loop
let familyNames = ["Park", "Lee", "Jong", "Kim", "Choi"]
var longFamilyNames = [String]()
for familyName in familyNames {
    if familyName.count > 3 {
        longFamilyNames.append(familyName)
    }
}
print(longFamilyNames)
// ["Park", "Jong", "Choi"]
```

Complexity: O(n), where n is the length of the sequence.

# Map

`Map` method returns an array that is filtered and manipulated. If you want to manipulate a value in an array for a new array, use `map`.

```swift
// Map
let prices: [Double] = [400, 350, 150, 225, 500]
let totalPricesWithTax = prices.map { $0 * 1.05 }
print(totalPricesWithTax)
// [420.0, 367.5, 157.5, 236.25, 525.0]


// For-in loop
let prices: [Double] = [400, 350, 150, 225, 500]
var totalPricesWithTax = [Double]()
for price in prices {
    totalPricesWithTax.append(price * 1.05)
}
print(totalPricesWithTax)
// [420.0, 367.5, 157.5, 236.25, 525.0]
```

# Reduce

Use `reduce` method to combine all values to create a single value, like the sum of all the value. An initial value and a combine closure is required when using `reduce`.

```swift
// Filter
let prices: [Double] = [420.0, 367.5, 157.5, 236.25, 525.0]
let totalPrice = prices.reduce(0.0, +)
print(totalPrice)
// 1706.25


// For-in Loop
let prices: [Double] = [420.0, 367.5, 157.5, 236.25, 525.0]
var totalPrice: Double = 0
for price in prices {
    totalPrice += price
}
print(totalPrice)
// 1706.25
```

```swift
let numbers = [1, 2, 3, 4]
let numberSum = numbers.reduce(0) { x, y in x + y }
print(numberSum)
// 10
```

Complexity: O(n), where n is the length of the sequence.

*When using filter, map, reduce methods, the returned array is immutable.*

Gist: [https://gist.github.com/imjhk03/7c11c541b2bc69ba75f558f90d00b48d](https://gist.github.com/imjhk03/7c11c541b2bc69ba75f558f90d00b48d)