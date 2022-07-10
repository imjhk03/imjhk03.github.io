---
title: HTML을 NSAttributedString으로 변환하기
layout: post
tags: [strings]
---

화면에 노출하는 데이터 중에 문자열을 다루는 데이터가 많다. 그중에 특정 문자열의 텍스트 스타일을 입혀서 보여주는 경우가 있는데, 보통 HTML을 가지고 포맷하는 경우가 많다. 이때, swift에서 ```HTML```을 ```NSAttributedString```으로 변환해서 보여줄 수 있다.

예를 들어 아래와 같이 HTML이 있다면:

```swift
"<b>Hello</b> World."
```

문자열을 ```NSAttributedString```으로 변환해서 ```UILabel```이나 ```UITextView```에 담으면 된다. 먼저 문자열을 ```Data``` 인스턴스로 변환한다.

```swift
let data = Data(html.utf8)
```

그러고 나서 ```NSAttributedString```으로 변환한다. 변환하는 과정 중에 유효하지 않을 수 있기 때문에 ```try?```를 사용한다.

```swift
if let attributedString = try? NSAttributedString(data: data, options: [.documentType: NSAttributedString.DocumentType.html, .characterEncoding: String.Encoding.utf8.rawValue], documentAttributes: nil) {
    stringLabel.attributedText = attributedString
}
```

## Extension
자주 사용할 것 같다면, 변환하는 과정을 extension으로 만들어 사용할 수 있다.

```swift
extension String {
    func asAttributedString() -> NSAttributedString? {
        guard let data = self.data(using: .utf8) else {
            return nil
        }
		
        return try? NSAttributedString(
            data: data,
            options: [.documentType: NSAttributedString.DocumentType.html, .characterEncoding: String.Encoding.utf8.rawValue],
            documentAttributes: nil
        )
    }
}

// Usage
let htmlString = "<p>Hello, <strong>world!</strong></p>"
label.attributedText = htmlString.asAttributedString()
```

## 스타일 입히기
HTML을 다루고 있기 때문에 CSS를 이용해서 텍스트 스타일을 입힐 수 있다. 스타일을 입힐 수 있는 HTML 코드를 작성해서 적용하면 된다. 색상과 폰트 사이즈도 조절하고 싶다면 파라미터를 사용해서 적용할 수도 있다.

```swift
func asAttributedString(size: CGFloat, color: UIColor) -> NSAttributedString? {
    let htmlTemplate = """
    <!doctype html>
    <html>
      <head>
        <style>
          body {
            font-family: -apple-system;
            font-size: \(size)px;
            color: \(color.hexString!);
          }
        </style>
      </head>
      <body>
        \(self)
      </body>
    </html>
    """
    
    guard let data = htmlTemplate.data(using: .utf8) else {
        return nil
    }
    
    guard let attributedString = try? NSAttributedString(
        data: data,
        options: [.documentType: NSAttributedString.DocumentType.html, .characterEncoding: String.Encoding.utf8.rawValue],
        documentAttributes: nil
    )
    else {
        return nil
    }
    
    return attributedString
}

extension UIColor {
    var hexString:String? {
        if let components = self.cgColor.components {
            let r = components[0]
            let g = components[1]
            let b = components[2]
            return  String(format: "#%02x%02x%02x", (Int)(r * 255), (Int)(g * 255), (Int)(b * 255))
        }
        return nil
    }
}

// Usage
label.attributedText = htmlString.asAttributedString(size: 18, color: .systemBlue)

```

## 주의점
주의해야할 점은 initializer를 호출할 때는 메인 쓰레드에서 호출해야 한다.

> The HTML importer should not be called from a background thread (that is, the options dictionary includes documentType with a value of html). It will try to synchronize with the main thread, fail, and time out. Calling it from the main thread works (but can still time out if the HTML contains references to external resources, which should be avoided at all costs). The HTML import mechanism is meant for implementing something like markdown (that is, text styles, colors, and so on), not for general HTML import.

> From [Apple Documents](https://developer.apple.com/documentation/foundation/nsattributedstring/1524613-init)

<br>
**참고**

[How to display HTML in UILabel and UITextView](https://sarunw.com/posts/how-to-display-html-in-uilabel-and-uitextview/)