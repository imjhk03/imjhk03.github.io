---
title: Use Sets for unique
layout: post
date: '2019-11-30 23:30:00'
tags: swift
---

If there is something you need to handle data unique, use **Sets** instead of **Array**.

Sets are like array, but there are some difference.

1. It is not stored in order. Use array instead.
2. No duplicate items can be stored. All items are unique.

```swift
let arrayOfFruit = ["Apple", "Banana", "Carrot", "Durian"]
// ["Apple", "Banana", "Carrot", "Durian"]
let setOfFruit = Set(["Apple", "Banana", "Carrot", "Durian"])
// {"Durian", "Banana", "Apple", "Carrot"}
```

If you write the code above in the playground, you can see that the value of set in unordered.

```swift
let setOfFruitDuplicate = Set(["Apple", "Banana", "Carrot", "Durian", "Apple"])
// {"Apple", "Carrot", "Durian", "Banana"}
```

Also, if you duplicate the value like above, you only get unique values. Above set would only include Apple, Banana, Carrot, Durian.

Because of having unique data, set is good for finding if there is any duplicated data. For example, counting the data would be good for finding if the data has duplicated data.
If there is a duplicated data, set would not include that, so it would have less count beside of the original data.

```swift
let arrayOfFruitDuplicate = ["Apple", "Banana", "Carrot", "Durian", "Apple"]
let setOfFruitDuplicate = Set(["Apple", "Banana", "Carrot", "Durian", "Apple"])

arrayOfFruitDuplicate.count == setOfFruitDuplicate.count
// it will return false
```