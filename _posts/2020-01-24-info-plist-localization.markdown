---
title: Info.plist Localization
layout: post
description: Localize Info.plist for description and application name, etc.
tags: iOS localize
---

Recently, one of our app got rejected by App Store Connect, because it needed to update `NSPhotoLibraryUsageDescription` content. There was not enough reason why our app need to access user's photo library. While solving the issue, I wanted to add localized message for it. This post will show how to localize of Info.plist.

## Add new Strings File
First, add a new `Strings File` naming "`InfoPlist`".

![Add new InfoPlist.strings](/assets/img/2020/01/24/image1.png)

## Enable Localization
While `InfoPlist.strings` file selected, press **Localize...** button in the file inspector panel.

![Localize Button](/assets/img/2020/01/24/image2.png)

After pressing the button, you will get localized files like below.

![Two new localize InfoPlist.strings file](/assets/img/2020/01/24/image3.png)

## Add Localized Message
Add localized description for each file that matches the language. You can change the app name to match the language of the country (`CFBundleDisplayName` and `CFBundleName`). The keys must be added to `Info.plist` file, or the localization will not work well.

```swift
// For App's Name
CFBundleDisplayName = "내튜브";
CFBundleName = "내튜브";
    
// For Photo Library Access
NSPhotoLibraryUsageDescription = "{Photo Library Access Message}";
```

## Test
Testing on the simulator, you need to change the language from the simulator's setting app. Testing on a real device is also the same way as simulator.

## Conclusion
While solving the issue of [5.1.1 Data Collection and Storage](https://developer.apple.com/app-store/review/guidelines/#data-collection-and-storage), I didn't know it has to give a very specific reason of access it. Make sure your app gives a very good description on why the app needs to access it.

### Reference
[About Information Property List Files](https://developer.apple.com/library/archive/documentation/General/Reference/InfoPlistKeyReference/Articles/AboutInformationPropertyListFiles.html)
