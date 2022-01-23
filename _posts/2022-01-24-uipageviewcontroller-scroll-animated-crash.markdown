---
title: UIPageViewController의 transitionStyle이 scroll일 경우, 크래시가 나는 버그 해결
layout: post
tags: iOS bugfix
---

이상하게 iOS 15에서 특정 페이지로 스크롤 할 때, 크래시가 발생하는 일이 생겼습니다. iOS 15 미만 기기에서는 발생하지 않았는데, iOS 15에서만 발생하여 iOS 15에서만 UIPageViewController가 내부적으로 특이하게 동작하는 것 같았습니다. 크래시가 발생하는 부분은 아래 코드 부분이었습니다.

```swift
setViewControllers:direction:animated:completion:
```

검색하다 보니 ```UIPageViewController```가 scroll type이면서 animation을 true로 할 경우, 내부 캐시 문제로 버그가 발생한다며 예전부터 있었던 버그인 것으로 보입니다 ([스택오버플로우](https://stackoverflow.com/a/12939384)). 여기서 해당 버그를 수정하는 방법이 2가지가 있습니다.

1. UIPageViewControllerTransitionStyleScroll을 사용하지 않는다
2. `setViewControllers:direction:animated:completion:` 을 호출할 때, `animated`을 `false`으로 지정한다.

아예 안 쓰는 거는 다른 페이지들도 영향이 가기 때문에, 2번째 방법을 사용하여 버그를 해결했습니다.

<br>
**참고**

[UIPageViewController navigates to wrong page with Scroll transition style](https://stackoverflow.com/a/12939384)