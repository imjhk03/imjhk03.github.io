---
title: UIView의 일부 모서리만 둥근 모서리를 설정하는 방법
layout: post
categories: [Tips]
tags: [uikit, ui development]
---

[ENG](https://imjhk03.github.io/posts/round-corners-specific-view/)

UIView에서 모서리를 둥글게 하려면 레이어의 **`cornerRadius`** 값을 설정하면 된다. 다음과 같이 사용하면 된다:

```swift
cornerView.layer.cornerRadius = 8
```

>서브뷰가 둥근 모서리로 잘리도록 하려면 다음을 한다: 'view.clipsToBounds = true'
{: .prompt-info}

하지만 상단 또는 하단으로만 모서리를 처리해야 한다면 어떻게 해야 할까? `CALayer`의 **`maskedCorners`** 속성을 설정해야 한다. 이렇게 둥근 모서리를 만들기 위한 extension을 만들 수 있습니다:

```swift
extension UIView {
    // Available from iOS 11.0
    func cornerRadius(_ corners: UIRectCorner..., radius: CGFloat) {
        layer.cornerRadius = radius
        
        var maskedCorners: CACornerMask = []
        let cornerSet = corners.isEmpty ? [.allCorners] : corners
        
        if cornerSet.contains(.allCorners) {
            maskedCorners = [.layerMinXMinYCorner, .layerMaxXMinYCorner, .layerMinXMaxYCorner, .layerMaxXMaxYCorner]
        } else {
            if cornerSet.contains(.topLeft) {
                maskedCorners.insert(.layerMinXMinYCorner)
            }
            if cornerSet.contains(.topRight) {
                maskedCorners.insert(.layerMaxXMinYCorner)
            }
            if cornerSet.contains(.bottomLeft) {
                maskedCorners.insert(.layerMinXMaxYCorner)
            }
            if cornerSet.contains(.bottomRight) {
                maskedCorners.insert(.layerMaxXMaxYCorner)
            }
        }
        
        layer.maskedCorners = maskedCorners
    }
}
```

이 extension은 다음과 같은 기능을 제공한다:
- 가변 매개변수를 사용하여 여러 모서리를 한 번에 설정할 수 있다
- `UIRectCorner`와 `CACornerMask` 간의 매핑을 통해 쉽게 원하는 모서리를 설정할 수 있다
- `reduce` 함수를 사용하여 선택된 모든 모서리를 하나의 마스크로 결합한다

아래 표는 `CACornerMask`와 모서리의 관계를 보여준다:

| CACornerMask        | 모서리               |
| ------------------- | ------------------- |
| layerMinXMinYCorner | 상단 왼쪽 모서리        |
| layerMaxXMinYCorner | 상단 오른쪽 모서리       |
| layerMinXMaxYCorner | 하단 왼쪽 모서리        |
| layerMaxXMaxYCorner | 하단 오른쪽 모서리       |

사용 예시:
```swift
// 모든 모서리를 둥글게
circleView.cornerRadius(.allCorners, radius: 50)

// 상단 모서리만 둥글게
circleView.cornerRadius(.topLeft, .topRight, radius: 50)

// 왼쪽 모서리만 둥글게
circleView.cornerRadius(.topLeft, .bottomLeft, radius: 25)

// 대각선 모서리만 둥글게
circleView.cornerRadius(.topLeft, .bottomRight, radius: 25)
```

<br>
**참고**
<br>
[cornerRadius](https://developer.apple.com/documentation/quartzcore/calayer/cornerradius){:target="_blank"}

[maskedCorners](https://developer.apple.com/documentation/quartzcore/calayer/maskedcorners){:target="_blank"}