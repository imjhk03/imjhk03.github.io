---
title: How to hide navigation bar when scrolling
layout: post
tags: iOS
---

In UINavigationController, there is a property called `hidesBarsOnSwipe` which hides its bars when scrolling or swipe down.

```swift
navigationController?.hidesBarsOnSwipe = true
```

We also need to set the view’s top anchor to superview’s top anchor. For example, there is a collection view inside a view controller. The collection view’s top anchor is related to Superview’s top anchor. If we don’t set it, we can’t show the navigation bar when scrolling or swipe up.

![Collection view's top anchor is related to superview's top anchor.](/assets/img/2021/04/16/image1.png)

![Animated image that shows navigation bar is hidden when scrolled.](/assets/img/2021/04/16/image2.gif)

Reference:<br>
[Apple Developer Documentation](https://developer.apple.com/documentation/uikit/uinavigationcontroller/1621883-hidesbarsonswipe)