---
title: How to show scrollbar over header or footer view
layout: post
tags: iOS
---

# Overview
After revisiting my old codes, I found some useful code that helped a bug. Although this bug is solved over iOS 13, if your project deployment target is iOS 12 or under, this code might be useful.

> Only from iOS 12 and under produces this bug. From iOS 13 and above, this bug doesn't occur.

# Scrollbar goes under the header or footer view
As the below image shows, the scrollbar is under the header view. It was a strange bug, and does not look good. But adding the below code helped the bug. It looks like the scrollbar layer's zPosition was lower than the header view layer's zPosition.

![The scrollbar is shown under the view, which makes a blank between the scrollbar](/assets/img/2021/05/12/image1.png)

```swift
func collectionView(_ collectionView: UICollectionView, willDisplaySupplementaryView view: UICollectionReusableView, forElementKind elementKind: String, at indexPath: IndexPath) {
    view.layer.zPosition = 0
}
```

<br>
Reference:<br>
[StackOverflow](https://stackoverflow.com/a/48088371)