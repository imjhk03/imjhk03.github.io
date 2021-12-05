---
title: Cannot find 'Something' in scope
layout: post
tags: cocoaPods
---

After updating some third party sdk version, there was a warning that can't find some library when building the project.

![An error that says 'Cannot find 'Analytics' in scope](/assets/img/2021/04/28/image1.png)

The issue was that some library was separated used, but after updating the sdk version, that library went into the main sdk library. (AnalyticsEventSelectContent -> FirebaseAnalytics) The library was redundant in the repo.

The way to fix this problem is to remove all coocapods and reinstall it.

```
pod deintegrate
pod install
```