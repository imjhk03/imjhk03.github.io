---
title: WWDC20 Dub Dub Diary, Part 3 - Code-along, New Components, and build for iPad
layout: post
categories: WWDC
---

Many sessions were also posted on the third day of WWDC20. In particular, sessions focused on game centers were concentrated, but they were not my interests because I wasn't developing game apps. I saw sessions on different topics, and there were also new types of sessions among.

## Widgets code-along

![Widgets code-along screenshot. Seeing session while coding along.](/assets/img/2020/08/06/image1.png)

There is a session where the process of developing Widgets is divided into three parts. Usually, the development process was with the presentation of the topic in one session. But this year, it was the first time that the development process was separated into a session. It was fun, and I was able to get a sense of how to develop Widgets. I didn't immediately understand whether this code was right because I wasn't developing the app from the beginning, but that didn't make a big deal. It was good to learn about Widgets by developing various topics. Sometimes having these types of sessions would be good for learning new features.

## iOS pickers, menus and actions

![Menus, Date and Time picker, Color picker](/assets/img/2020/08/06/image2.png)

The interesting sessions were the new components from iOS 14. Using Menu, we can provide users with a more compacted and UX-friendly choice, choosing dates and times is easy through Date and time picker, and selecting wider colors through Color picker.

### Color Picker

With UIColorPickerViewController, you can display a color-selectable screen and select colors by three types: Grid, Spectrum and Sliders. With the new view controller, you can select or make a color easy. And it is shown in a sheet form, so it does not cover the entire screen.

### Date Picker

When selecting the date and time, we used the picker view. But now we can use the dedicated picker view in iOS 14. The date of the year can be easily found and selected on the screen, and you can use keyboard to input time. You can limit only the date or time, and in iOS you can display the date and time in an inline style.

### Menus

In `NavigationBar`, easy pop backwards can be made through Menu. It is designed to replace Action sheets and popovers and can be used for various purposes such as selection and navigation. UI became simple by putting all the features that had to be shown on the screen to the menu. Below is an example from the Folders app.

![Folders app without menu and with menu. Much cleaner screen.](/assets/img/2020/08/06/image3.png)

The new components supports multi-platform. It would be a good idea to take this opportunity to develop an app that can be developed not only on iPhones but also on iPads and even on macOS.

### Build for iPad

![Shortcuts App in iPad. There is a Sidebar to the left.](/assets/img/2020/08/06/image4.png)

While talking about multi-platform, I watched an iPad-related session. After watching this session, I wanted to develop a very attractive iPad app. In particular, through this session, there is a very detailed explanation of how to develop an iPad app. Using `UISplitViewController`, you can configure the screen to be shown on the iPad and the screen to be shown in iPhone or compact mode. You can also create a list using `UICollectionView` to develop a new Sidebar. Through this session, I felt it was easy to create an iPad app and thought it would be good to implement a new feature called Sidebar together.

## Wrap up

The more I listened to the sessions, the stronger I felt Apple's willingness to support multi-platform. Maybe it was for the upcoming Apple Silicon and to make a general use of SwiftUI.
