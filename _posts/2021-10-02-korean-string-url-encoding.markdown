---
title: 한글 들어간 url string을 인코딩하는 방법
layout: post
tags: [swift, strings, url]
---

문자열(이후 string)을 URL로 변환하여 사용하는 경우가 있는데, string 값에 한글 혹은 공백 같은 값이 들어갈 경우 nil 값이 반환된다. 퍼센트 인코딩(percent-encoding)을 해야 올바른 url로 변환할 수 있는데, 스위프트 string에서 `addingPercentEncoding(withAllowedCharacters:)` 을 사용하면 된다. 파라미터로 `.urlQueryAllowed`을 넣는데, 사용하려는 URL 형식에 맞게 `CharacterSet`에 있는 값들 중에 사용하면 된다.

```swift
import Foundation

let urlString = "https://www.google.com/search?q=애플 스위프트"
if let encodedString = urlString.addingPercentEncoding(withAllowedCharacters: .urlQueryAllowed) {
    print(encodedString)
    // Prints "https://www.google.com/search?q=%EC%95%A0%ED%94%8C%20%EC%8A%A4%EC%9C%84%ED%94%84%ED%8A%B8"
}

if let encodedURL = URL(string: encodedString) {
    print(encodedURL)
    // Prints "https://www.google.com/search?q=%EC%95%A0%ED%94%8C%20%EC%8A%A4%EC%9C%84%ED%94%84%ED%8A%B8"
}
```

> 🚧 주의
> 
> 여기서 주의할 점은 이미 퍼센트 인코딩되어 있는 string에 사용하면 안 된다. 사용하게 된다면 퍼센트 인코딩되어 있는 string에 있는 퍼센트 문자가 또 퍼센트 인코딩하는 것을 발생할 수 있다.

<br/>

참고

[Apple Developer Documentation](https://developer.apple.com/documentation/foundation/nsstring/1411946-addingpercentencoding)