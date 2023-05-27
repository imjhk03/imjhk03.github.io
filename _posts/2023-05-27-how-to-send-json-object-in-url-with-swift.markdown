---
title: 스위프트에서 URL에 JSON을 파라미터로 보내는 방법
layout: post
category: [Tips]
tags: [url]
---

스위프트에서 `wkwebview`로 url 로드할 때, 아래와 같이 json 형태의 쿼리 파라미터를 담아서 호출해야 하는 경우가 있을 수 있다.
```
http://<host>?params=<JSON object>
```

이런 경우에는 보내고자 하는 JSON 객체를 인코딩해서 담아서 로드하면 된다.
```swift
let jsonString = """
{
    \"key\":\"value",
    \"키\":\"여러 가지 값들을\"
}
"""
if
    let urlEncodedJson = jsonString.addingPercentEncoding(withAllowedCharacters: .urlQueryAllowed),
    let url = URL(string: "\(urlString)?params=\(urlEncodedJson)")
{
    webView.load(URLRequest(url: url))
}
```

**참고**
<br>
[How to send Json as parameter in url using swift](https://stackoverflow.com/a/29895983)