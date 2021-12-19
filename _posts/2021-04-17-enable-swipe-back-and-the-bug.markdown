---
title: Enable swipe back and the bug
layout: post
tags: iOS
---

## Enable swipe back when navigation bar is hidden
In iOS, we can swipe back(left to right) to pop the view controller and navigate back. This is only available when the navigation bar is shown. If we want to swipe back even if the navigation bar is hidden, add below code at the root view.

```swift
override func viewDidLoad() {
    super.viewDidLoad()

    ...

    navigationController?.interactivePopGestureRecognizer?.delegate = nil
}
```

## The bug
But be careful when using it. There is a bug, which is when we swipe back at the very root view controller. Nothing happens because there is no view controller to pop. But then, nothing happens when tapping anywhere. The view controller does not response at all. If we swipe back again, a strange view appears from the left as we swipe. After this happens, the root view controller now gets response. Below image shows the strange behavior.


![A list of movie poster and tapped one of them to see the detail. Scrolls down to hide navigation bar, and successfully swipes back to pop view controller and navigate back. Scrolls down to the middle of the list and swipes back. Nothing happens but tapping to one of the list does not response. Swipes back and from the right shows a detail view controller. After that, the view controller response normally.](/assets/img/2021/04/17/image1.gif)

To prevent this strange behavior, we need to handle the ```interactivePopGestureRecognizer``` to be enabled or disabled. This can be in the root view controller or implement ```UINavigationControllerDelegate``` to the root view controller. It depends on how your root view controller is structed.

```swift
// UINavigationControllerDelegate Implement
func navigationController(_ navigationController: UINavigationController, didShow viewController: UIViewController, animated: Bool) {
    interactivePopGestureRecognizer?.isEnabled = viewControllers.count > 1
}


// or the root view controller
override func viewDidAppear(_ animated: Bool) {
    super.viewDidDisappear(animated)

    navigationController?.interactivePopGestureRecognizer?.isEnabled = navigationController?.viewControllers.count ?? 0 > 1
}
```

Adding this will fix the problem. Maybe the swipe back gesture was troubling, even though the navigation controller does not have view controllers to pop. One of our apps was having some trouble about the view freezing, and we had produced this bug somehow. And it was related to this bug. Adding this code had solved the freezing bug.

<br>
Reference:<br>
[Apple Documentation](https://developer.apple.com/documentation/uikit/uinavigationcontroller/1621847-interactivepopgesturerecognizer)
<br>
[StackOverflow](https://stackoverflow.com/questions/34698018/interactivepopgesturerecognizer-corrupts-navigation-stack-on-root-view-controlle)