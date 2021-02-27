---
title: Add Refresh Control to Collection View
layout: post
description: How to add pull-to-refresh control to scroll views
tags: iOS
---

I've been currently rebuilding a project that I'm working on, and there were some UI issues when refreshing datas. `UIRefreshControl` was implied for pull-to-refresh style, but the project deployment target was iOS 8 and some old codes were left. After changing the code, I wanted to write a post about it. So this post will show how to add pull-to-refresh style to collection view or other scroll views.

## UIRefreshControl starting in iOS 10

In pre-iOS 10, `UIRefreshControl` was added as a subview like below.

```swift
let refreshControl = UIRefreshControl()
refreshControl.addTarget(self, action: #selector(reloadCurrentView), for: UIControlEvents.valueChanged)
collectionView.addSubview(refreshControl)
```

Starting in iOS 10, you can add a `UIRefreshControl` as a property to `UIScrollView`.

```swift
let refreshControl = UIRefreshControl()
refreshControl.addTarget(self, action: #selector(handleRefresh), for: .valueChanged)
collectionView.refreshControl = refreshControl
    
@objc func handleRefresh() {
    // Update your content…
    
    // Dismiss the refresh control.
    DispatchQueue.main.async {
        self.collectionView.refreshControl?.endRefreshing()
    }
}
```

## UIRefreshControl Attributes

You can change the tint color and add title text while displaying the refresh control.

```swift
let refreshControl = UIRefreshControl()
let title = "Pull to refresh"
refreshControl.tintColor = .yellow
refreshControl.attributedTitle = NSAttributedString(string: title, attributes: [.foregroundColor : UIColor.yellow])
refreshControl.addTarget(self, action: #selector(handleRefresh), for: .valueChanged)
tableView.refreshControl = refreshControl
```

If you can handle when the data is fetched or while fetching, you can change `attributedTitle` like below.

![Pull to refresh and refreshing image](/assets/img/2020/01/19/image1.jpeg)

## Wrap up

Add `UIRefreshControl` if you need a pull-to-refresh control for your list page or more. Using it and customizing is easy.


### Reference
[UIRefreshControl - UIKit][appleDoc]{:target="_blank"}

[appleDoc]: https://developer.apple.com/documentation/uikit/uirefreshcontrol 