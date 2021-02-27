---
title: WWDC20 Dub Dub Diary, Part 2 - Widget, App clips, and SwiftUI
layout: post
categories: WWDC
---

On the second day of WWDC20, sessions were uploaded all at once and could be viewed immediately without waiting. Before this year, there were sessions by time and place, so waiting and listening a session took a long time. But this year, the sessions were released all at once, so no more waiting anymore.

When listening to sessions through developer apps, it was interesting that this year there was a function to copy code script. While listening to the session, typing all the parts of code described in the session took long time, but now just copy them right away by pressing the copy button. The codes are showed along by time.

![Code Copy Button on Developer App](/assets/img/2020/07/27/image1.png)
Image of button copying developer app code

Also, when looking through the list of sessions, there were surprisingly many sessions for only 10 to 20 minutes. Previously, sessions were usually prepared on a 40-minute basis, so it was very good to have such short sessions. Short sessions and smooth presentation were very good to provide only the key points of the subject.

## Widget, App Clips, and SwiftUI

Among the sessions, the one I wanted to hear was the new Widgets and App Clips on iOS 14. In particular, what I felt while watching these sessions was that now studying SwiftUI is much more important this year.

Widgets and App Clips can be developed with SwiftUI (App Clips can also be developed with Swift and/or Objective-C). Developing these features not using UIKit, but only SwiftUI can be easy to create view or support multi-platform. But I think further more, Apple is planning to do more things with SwiftUI. In other words, SwiftUI is likely to have major part in the future.

![Rock Paper Scissor Game SwiftUI](/assets/img/2020/07/27/image2.png)
Rock Paper Scissor Game SwiftUI App

Although I haven't studied SwiftUI deeply yet, I've developed a [game app](https://github.com/imjhk03/RockPaperScissors){:target="_blank"} that was created simply using SwiftUI this year. At first, it was not difficult to make a screen, but it was really unfamiliar with how data was used and how the screen changed depending on the condition. I had to look for new terms and wondered if it was right to write them like this. I only had a short experience of studying it, so I thought I should study SwiftUI properly this year. If you look at the ["*Integration of SwiftUI*"](https://developer.apple.com/videos/play/wwdc2020/10119/){:target="_blank"} session, you can see the overall SwiftUI content and development process.

### Build an app entirely with SwiftUI
Some of the new feature of SwiftUI is that you can build an app entirely with SwiftUI. On iOS 14, with a new ```App``` protocol and new ```@main``` annotation attribute, the entry point for the app is finish. No app delegate.

```swift
@main
struct BooksApp: App {
    var body: some Scene {
        WindowGroup {
            ContentView()
        }
    }
}
```

As you can see, it is very familiar with the view how SwiftUI builds. For example, it looks similar with the ContentView SwiftUI code.

```swift
import SwiftUI

struct ContentView: View {
    
    @State private var book = Book()
    
    var body: some View {
        VStack(spacing: 20) {
            Text("📚")
                .font(.system(size: 24))
            Text(book.name)
                .foregroundColor(.white)
                .font(.title)
        }
    }
}
```

### Link
One of the interesting part is opening a link in SwiftUI. Using ```Link```, URL can open to a web browser or function as a app link.

```swift
let websiteURL: URL = ...

Section {
    Link(destination: websiteURL) {
        Label("Go to website")
    }
}

```

There are more things that are announced in the session ["*What's new in SwiftUI*"](https://developer.apple.com/videos/play/wwdc2020/10041/){:target="_blank"}. I also recommend this session also.

## Wrap Up
What I noticed that Apple is really building up SwiftUI, to use and support multi-platform, build an entire app, etc. Now is the time to study SwiftUI and build a small app trying new things with it. Wish I could make an app with SwiftUI only, but also do some things that only does with UIKit. Another new stuff to study this year 😆
