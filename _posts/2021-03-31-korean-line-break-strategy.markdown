---
title: iOS 14부터 UILabel에서 한글 사용할 때 줄바꿈 이쁘게 하기 (lineBreakStrategy)
layout: post
author: Joohee Kim
tags: iOS
---

iOS에서 `UILabel`에 한글을 사용할 때, 줄 바꿈이 이쁘게 되지 않아 문제가 됐었는데, 찾다 보니 iOS 14에서부터 한글 사용할 때 줄 바꿈이 이쁘게 할 수 있는 설정이 있다고 해서 테스트해봤다.

# iOS 13
아래 스크린샷과 같이 한글이 길 경우에는 줄 바꿈이 매끄럽지 않다. 위의 레이블은 `attributedText`이고 아래 레이블은 일반 텍스트이다.

![iOS 13 UILabel 한글 줄바끔이 매끄럽지 않다.](/assets/img/2021/03/31/image1.png)

"표준시", "로 ~"

"있습", "니다. ~"

깔끔하게 줄 바꿈이 되지 않는 것을 보일 수 있다. `lineBreakMode` 프로퍼티를 이용해서 값을 `.byWordWrapping`으로 설정해도 한글일 때는 잘 먹히지 않는다.

# iOS 14 (lineBreakStrategy)
`lineBreakStrategy`라는 프로퍼티가 있는데, 정의는 설명되어 있지 않다. 찾다 보니 기존에는 `pushOut` 값이 있는데, iOS 14에서부터 `hangulWordPriority`, `standard` 값이 추가되었다. 영어 이름으로 hangul이 있다는 게 신기했는데, 해당 값을 적용하면 잘 나온다. `NSParagraphStyle`를 이용해서 설정하고 사용하면 된다.


```swift
extension NSParagraphStyle {

    
    public struct LineBreakStrategy : OptionSet {

        public init(rawValue: UInt)

        
        @available(iOS 9.0, *)
        public static var pushOut: NSParagraphStyle.LineBreakStrategy { get }

        
        @available(iOS 14.0, *)
        public static var hangulWordPriority: NSParagraphStyle.LineBreakStrategy { get }

        
        @available(iOS 14.0, *)
        public static var standard: NSParagraphStyle.LineBreakStrategy { get }
    }
}
```

```swift
attributedLabel.numberOfLines = 0
attributedLabel.backgroundColor = .secondarySystemBackground
        
let font = UIFont.systemFont(ofSize: 17, weight: .semibold)
let paragraphStyle = NSMutableParagraphStyle()
paragraphStyle.lineSpacing = 6
// 한글 줄바꿈 적용
if #available(iOS 14.0, *) {
    paragraphStyle.lineBreakStrategy = .hangulWordPriority
}
        
let attributes: [NSAttributedString.Key: Any] = [
    .font: font,
    .foregroundColor: UIColor.systemBlue,
    .paragraphStyle: paragraphStyle
]
attributedLabel.attributedText = NSAttributedString(string: text, attributes: attributes)
```

![iOS 14에서 lineBreakStrategy를 사용하면 한글 줄바꿈이 매끄럽다.](/assets/img/2021/03/31/image2.png)

여기서 특이한 것은, iOS 14에서는 특별한 설정 없이 일반 텍스트일 때는 잘 나온다. iOS 14부터는 기본적으로 언어에 따라서 줄 바꿈 하도록 되어 있는 것 같은데 맞는지는 다른 언어를 사용해보지 않아서 잘 모르겠다.

그리고 또 한 가지, paragraphStyle을 이용해서 텍스트에 attribute를 추가하면 줄 바꿈이 이상하게 나오는데, paragraphStyle을 사용하지 않으면 줄 바꿈이 잘 된다. 즉, paragraphStyle을 사용 안 하면 한글 줄 바꿈이 잘 나온다.

이제 iOS 14에서부터 아주 매끄럽게 한글 줄 바꿈이 잘 나오는 것을 확인할 수 있다. 만약 paragraphStyle을 사용한다면, lineBreakStrategy 값을 설정해서 적용하면 잘 나온다.

<br>
참고: <br>
[NSParagraphStyle.LineBreakStrategy](https://developer.apple.com/documentation/uikit/nsparagraphstyle/linebreakstrategy?changes=_8)