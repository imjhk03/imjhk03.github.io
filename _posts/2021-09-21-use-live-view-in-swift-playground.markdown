---
title: Use Live View in Swift Playground
layout: post
tags: [swift playground, developer tools]
---

Recently I've been using Swift Playground app on iPad for studying Swift language. Personally, I think the playground app is one of the best apps for learning swift. While writing swift code is enough, there are sometimes to write UI related code. Here is how to use swift playground live view to show view.

Add the below code to the swift playground. Importing PlaygroundSupport gives us to use live playground previewing.

```swift
import PlaygroundSupport
import UIKit

class ViewController: UIViewController {

    override func viewDidLoad() {
        super.viewDidLoad()
        // Write something
    }

}
```

After writing some view-related code, we need to add a code to create and present the view controller. Adding the view controller to the playground live view will do the work.

```jsx
let controller = ViewController()
PlaygroundPage.current.liveView = controller
```

If you build and run that playground, the view will appear on the assistant editor. Below is the screenshot of swift playground on iPad. We can also use navigation controller to push and pop a detail view.

![Swift playground app showing a view controller at the right side.](/assets/img/2021/09/21/image1.png)

Try using this code to write a simple UI related code or more, especially on iPad. It is one of a best tool for learning swift. Try it out!