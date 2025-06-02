---
title: SwiftUI에서 TextField 세로로 쓰기
description: TextField의 axis 속성으로 세로 스크롤 구현하기
layout: post
categories: [Tips]
tags: [swiftui]
image:
    path: /assets/img/2025/06/02/image1.png
---

>iOS 16.0 이상에서 사용할 수 있다.
{: .prompt-info}

SwiftUI에서 TextField의 텍스트가 영역을 넘어설 때 세로로 스크롤되게 하려면 `axis` 파라미터를 `.vertical`로 설정하면 된다.

## 기본 사용법

```swift
import SwiftUI

struct ContentView: View {
    @State var text = ""

    var body: some View {
        VStack {
            TextField(
                "여기에 텍스트를 입력하세요",
                text: $text,        // 세로 스크롤 설정
                axis: .vertical
            )
            .lineLimit(5...)       // 최소 5줄 이상 표시
        }
        .frame(width: 200)
    }
}
```

## 주요 특징
- 텍스트가 길어지면 자동으로 세로 스크롤이 생성됨
- `lineLimit(_:)` modifier로 최소/최대 줄 수 지정 가능
- 키보드의 return 키로 새로운 줄 추가 가능

![텍스트필드에 긴 문장이 세로로 표시된 모습](/assets/img/2025/06/02/image1.png)