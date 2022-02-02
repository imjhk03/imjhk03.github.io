---
title: How to deal with scroll view content size with storyboard
layout: post
author: Joohee Kim
description: Create scroll view properly with storyboard
tags: [iOS, ui development, storyboard]
---

`UIScrollView` is very useful when presenting content that are larger than a single screen. I've been using it to support iPhone SE users or iPhone 8 users to let them scroll and see contents.

I prefer using Interface Builder to add or modify `UIScrollView` that is already used in a `UIViewController`. At first, it was fustrating how to set the content size and other errors when using `UIScrollView` (especially scrollable content size ambiguity error). This post is going to describe how to use `UIScrollView` in `Storyboard` that helped me alot.

## Steps

![Example Storyboard Scene](/assets/img/2020/01/10/image1.png)

This is an example of one `UIViewController` that has a `UIScrollView`. When it was first made, it was matched with iPhone 5. But after setting the storyboard view as iPhone 11, the layouts are not setted well. It's because the `UIScrollView` layouts were not properly setted before. Let's fix this problem.

### Step 1
The `UIView` inside `UIScrollView` need below constraints.

1. Leading, trailing, top and bottom constraints (all zero).

### Step 2
Add a `UIView` that contains the `UIScrollView`. This view needs below constraints.

1. Leading, trailing, top and bottom constraints (all zero to Superview).

![Added UIView and constraints](/assets/img/2020/01/10/image2.png)

### Step 3
Add equal height, equal width contraints to the UIView inside `UIScrollView` to the `UIView` that contains `UIScrollView`.

1. Add equal height, equal height (to the `UIView` that contains `UIScrollView`). Set priority to low for equal height.
2. Check the `UIScrollView`'s constraints. `UIScrollView` constraints needs to be setted as below.
    2-1. Leading, trailing, top and bottom constraints (all zero to Superview).

![The final constraints](/assets/img/2020/01/10/image3.png)


After the steps, you can run different devices and see it will scroll through all content. And also in the storyboard you will no longer see any red error like 'Scrollable Content Size Ambiguity Error'. 👏

If you still can't scroll, try adding `scrollView.contentSize` in `viewWillLayoutSubviews` function.

```swift
override func viewWillLayoutSubviews() {
    super.viewWillLayoutSubviews()
    scrollView.contentSize = CGSize(width: self.view.frame.size.width, height: 600)
}
```

## Conclusion
Even though large phones are common, there are still users that use small phones. It's best to support all devices using auto layout.

Thanks for reading!


### Reference
[Calculating contentSize for UIScrollView when using Auto Layout][stackOverFlow]{:target="_blank"}

[stackOverFlow]: https://stackoverflow.com/questions/38948904/calculating-contentsize-for-uiscrollview-when-using-auto-layout 