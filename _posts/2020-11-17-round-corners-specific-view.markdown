---
title: How to round corners specifically on a UIView
layout: post
author: Joohee Kim
description: How to round corners specifically on a UIView
categories: Tips
tags: [iOS, ui development]
---

To round a corner on a UIView, you can set the layer's **`cornerRadius`** value. Simply use it like this:

```swift
cornerView.layer.cornerRadius = 8
cornerView.layer.clipsToBound = true    // to make the corner work, clipsToBound must be true
```

But what if you need to corner only to top view or bottom? We need to set the layer's **`maskedCorners`** property. You can make an extension for making a rounded corner like this:

```swift
extension UIView {
    // available from iOS 11.0
    func roundCorners(_ corners: UIRectCorner, radius: CGFloat) {
        self.layer.cornerRadius = radius
        var cornerMask = CACornerMask()
        if corners.contains(.topLeft) { cornerMask.insert(.layerMinXMinYCorner) }
        if corners.contains(.topRight) { cornerMask.insert(.layerMaxXMinYCorner) }
        if corners.contains(.bottomLeft) { cornerMask.insert(.layerMinXMaxYCorner) }
        if corners.contains(.bottomRight) { cornerMask.insert(.layerMaxXMaxYCorner) }
        self.layer.maskedCorners = cornerMask
    }
}
```