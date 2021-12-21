---
title: How to show build times in Xcode
layout: post
categories: [Tips, Xcode]
tags: xcode
---

You can show how long the project build in Xcode by entering below command in ```Terminal.app```.

```zsh
defaults write com.apple.dt.Xcode ShowBuildOperationDuration YES
```

After entering the command, if you build Xcode it will show the build time on the activity viewer.

![The build time will show on activity viewer](/assets/img/2021/01/17/image1.png)

If you change YES to NO, the build time will not show in Xcode.
