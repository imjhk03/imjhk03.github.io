---
title: How to create a view controller from xib
layout: post
author: Joohee Kim
description: Create xib file for view controller
tags: [iOS, ui development, view controllers, storyboard]
---

Creating a new view controller was easy. I've used to create a view controller from Storyboard, and instantiated in code. Although this way is easy, so many view controllers in a Storyboard can increase compiling time. While working with new people in iOS team, there was a way to create a view controller from xib. Let's see how to do it.

## 1. Create a new xib file
First, create a new empty xib file and name it. Then, add a UIView to the empty place. Add other components you need.

![An empty xib file having a UIView](/assets/img/2020/11/09/image1.png)

## 2. Create a swift file
Create a swift file that associates with the new view controller xib file. Here are the start code for example.

```swift
import UIKit

final class MainViewController: UIViewController {
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
    }
    
}
```

## 3. Link both files
Now let's link both files. In the xib file, the *File's Owner* is the swift file name. Put the view controller swift file name at the **File's Owner** Custom Class > Class.

![UIView xib file's owner is the swift file name](/assets/img/2020/11/09/image2.png)

After that, you can add IBOutlets to the swift file. As you add the ```IBOutlets```, you can see the object is **File's Owner** below the screenshot. I added a new collection view and at ```viewDidLoad``` function, I set the collection view's background color to red.

![IBOutlet's object is file's owner](/assets/img/2020/11/09/image3.png)

```swift
import UIKit

final class MainViewController: UIViewController {
    
    @IBOutlet weak var collectionView: UICollectionView!
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        collectionView.backgroundColor = .red
    }
    
}
```

## 4. Get the view controller
After everything is all set, let's get the view controller and use it. It is very simple.

```swift
let previousVC = MainViewController()
self.present(previousVC, animated: true, completion: nil)   // present or push as you wish
```

But when you build and run, the app will crash. And the console log will show the error like below.

```
terminating with uncaught exception of type NSException
*** Terminating app due to uncaught exception 'NSInternalInconsistencyException', reason: '-[UIViewController _loadViewFromNibNamed:bundle:] loaded the "MainViewController" nib but the view outlet was not set.'
```

Why is this error occurring? We have forgotten to link the ```UIView``` itself to the file's owner. This error was the common error I had while creating a xib for view controller. So let's solve this problem.

Go to the xib file and at the **File's Owner** 's Connection Inspector, link the view object to the view in the Document Outline. See the image below for more details.

![Linking the view from Connection Inspector to the view in Document Outline](/assets/img/2020/11/09/image4.png)

Now build and run, and your app won't crash and show the view controller successfully.

![A new view controller is successfully presented](/assets/img/2020/11/09/image5.png)

<br>

### How to get the view controller from Storyboard?
Below code is how to create a view controller from Storyboard.

```swift
let storyboard: UIStoryboard = UIStoryboard(name: "Main", bundle: nil)
let authenticationVC: UIViewController = storyboard.instantiateViewController(withIdentifier: "AuthenticationVC") as UIViewController
```

<br>

## Summary
In this post, I've explained how to create a view controller from xib file. From [Apple's document](https://developer.apple.com/documentation/uikit/uiviewcontroller){:target="_blank"}, if you specify the views for a view controller using a xib(nib) file, you can't define segues or relationships between view controllers. Use wisely when creating a view controller from xib rather than storyboard.