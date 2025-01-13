---
title: Swift의 문자열 보간법 이해하기
layout: post
tags: [strings]
---

## 문자열 보간법이란?
문자열 보간(String interpolation)은 고정 텍스트를 변수, 표현식, 값과 혼합하여 문자열을 구성할 수 있는 Swift의 강력한 기능이다. Swift의 문자열 보간 시스템은 타입에 안전하며 효율적이고 `StringInterpolation` 프로토콜을 통해 고도로 사용자 정의할 수 있다.

```swift
let name = "John"
let greeting = "Hello, \(name)"

let a = 15
let b = 30
let calculation = "The sum of \(a) and \(b) is \(a + b)"

let isAdult = age >= 18
let status = "Status: \(isAdult ? "Adult" : "Minor")"
```

## Swift 5에서 달라진 점
Swift 5는 [SE-0228](https://github.com/apple/swift-evolution/blob/master/proposals/0228-fix-expressiblebystringinterpolation.md)을 통해 문자열 보간 기능을 크게 개선했다. Swift 3 이후로 더 이상 사용되지 않던 이전 버전의 `ExpressibleByStringInterpolation` 프로토콜을 현대화하고, 더 강력한 기능들을 추가했다.

기존 숫자나 날짜 타입에 적용하면 조금 더 보기 쉽고 읽기 쉽게 만들 수 있다. 예를 들면, 오늘 날짜를 출력하도록 하면 아래와 같이 나타난다.
```swift
print("오늘 날짜는 \(Date())")
// Prints "오늘 날짜는 2025-01-13 06:57:57 +0000"
```

우리가 실제로 날짜를 읽을 때와 다른 형식으로 나타난다. 하지만 `String.StringInterpolation` 프로토콜을 확장하면 우리가 원하는 형태로 날짜를 표현할 수 있다.
```swift
extension String.StringInterpolation {
    mutating func appendInterpolation(_ date: Date) {
        let formatter = DateFormatter()
        formatter.dateStyle = .full
        formatter.locale = Locale(identifier: "ko_KR")		// 한글

        let dateString = formatter.string(from: date)
        appendInterpolation(dateString)
    }
}

print("오늘 날짜는 \(Date())입니다.")
// Prints "오늘 날짜는 2025년 1월 13일 월요일입니다."
```

여기서 한 걸음 더 나아가, 사용자가 날짜 스타일을 직접 지정할 수 있도록 만들 수 있다.
```swift
extension String.StringInterpolation {
    mutating func appendInterpolation(format date: Date, style: DateFormatter.Style) {
        let formatter = DateFormatter()
        formatter.dateStyle = style
        formatter.locale = Locale(identifier: "KOR_KR")

        let dateString = formatter.string(from: date)
        appendInterpolation(dateString)
    }
}

print("오늘 날짜는 \(format: Date(), style: .long)입니다.")
// Prints "오늘 날짜는 2025년 1월 13일입니다."
```

### 디버깅을 위한 활용
문자열 보간은 커스텀 타입의 디버깅에도 유용하게 사용할 수 있다. 예를 들어, API 응답으로 받은 데이터를 JSON 형태로 예쁘게 출력할 수 있다.
```swift
struct User: Encodable {
    var name: String
    var age: Int
}

extension String.StringInterpolation {
    mutating func appendInterpolation<T: Encodable>(debug value: T) {
        let encoder = JSONEncoder()
        encoder.outputFormatting = .prettyPrinted

        if let result = try? encoder.encode(value) {
            let str = String(decoding: result, as: UTF8.self)
            appendLiteral(str)
        }
    }
}

let user = User(name: "John", age: 25)
print("Logged user: \(debug: user)")
/*
Prints "Logged user: {
  "name" : "John",
  "age" : 25
}"
*/
```

### 조건부 텍스트 처리하기
`@autoclosure`를 활용하면 조건에 따라 다른 텍스트를 출력하는 보간법을 만들 수 있다.`@autoclosure`를 사용하면 간단한 값이나 복잡한 함수를 기본값으로 사용할 수 있다는 의미다. 이 [블로그](https://ericasadun.com/2018/12/12/the-beauty-of-swift-5-string-interpolation/){:target="_blank"}에서 예를 보면 삼항 연산자(ternary expression)을 대신해서 조건에 출력하는 값을 조절할 수 있다.
```swift
extension String.StringInterpolation {
    mutating func appendInterpolation(if condition: @autoclosure () -> Bool, _ literal: StringLiteralType) {
        guard condition() else { return }
        appendLiteral(literal)
    }
}

let isSignature = true
print("Swift Coffee \(if: isSignature, "*")")
print("Swift Coffee \(isSignature ? "*" : "")")

```

## 문자열 보간으로 커스텀 타입 만들기
문자열 보간에서 다양하게 원하는 형식으로 문자열을 출력할 수 있는 것을 보았는데, 문자열 보간을 이용해서 사용자 지정 타입도 만들 수 있다. `ExpressibleByStringInterpolation` 프로토콜과 함께 `StringInterpolation` 프로토콜을 사용하면 컴파일 타임의 타입 안전성을 보장하면서도 표현력이 풍부한 API를 만들 수 있다.

커스텀 타입을 만들 때는 몇 가지 규칙을 따라야 한다:
1. **프로토콜 준수:** `ExpressibleByStringLiteral`, `ExpressibleByStringInterpolation`을 반드시 구현해야 하며, 필요한 경우 `CustomStringConvertible`도 구현한다.
2. **중첩 구조체:** 타입 내부에는 `StringInterpolationProtocol`을 준수하는 `StringInterpolation` 구조체가 필요하다.
3. **필수 구현:**
   * 중첩된 구조체에는 대략적인 데이터 양을 알려주는 두 개의 정수를 허용하는 이니셜라이저가 있어야 한다.
   * 또한 하나 이상의 `appendLiteral()` 메서드와 하나 이상의 `appendInterpolation()` 메서드를 구현해야 한다
   * 기본 타입에는 문자열 리터럴과 문자열 보간으로 생성할 수 있는 두 개의 이니셜라이저가 있어야 한다.

이러한 구조가 필요한 이유는 다음과 같다:
* 중첩 구조체를 사용하면 기본 타입이 보간 구현 세부사항으로 복잡해지지 않는다.
* 임시 데이터를 저장하는 공간을 제공한다. (Swift Evolution 제안서는 이를 '스크래치패드(scratchpad)'라고 부른다)
* 예상 데이터 양을 알려주어 메모리를 효율적으로 할당할 수 있다.
* 기본 타입에 이니셜라이저가 두 개 있으면 문자열 리터럴(“hello, world!”)과 문자열 보간(“hello, \(name)!”)을 모두 처리할 수 있다.

### AttributedString 만들기 예제
실제로 어떻게 사용되는지 AttributedString을 만드는 예제를 통해 살펴보자:
```swift
struct StyledText: ExpressibleByStringInterpolation {
    // The final attributed string
    let attributedString: NSAttributedString
    
    // Required by ExpressibleByStringInterpolation
    init(stringInterpolation: StringInterpolation) {
        self.attributedString = stringInterpolation.attributedString
    }
    
    // Required for string literals
    init(stringLiteral value: String) {
        self.attributedString = NSAttributedString(string: value)
    }
    
    // Custom string interpolation implementation
    struct StringInterpolation: StringInterpolationProtocol {
        // Storage for building the attributed string
       	var attributedString: NSMutableAttributedString
        
        // Required by StringInterpolationProtocol
        init(literalCapacity: Int, interpolationCount: Int) {
            self.attributedString = NSMutableAttributedString()
        }
        
        // Handle plain text
        mutating func appendLiteral(_ literal: String) {
            let attrs = NSAttributedString(string: literal)
            attributedString.append(attrs)
        }
        
        // Bold text interpolation
        mutating func appendInterpolation(bold text: String) {
            let attrs = NSAttributedString(
                string: text,
                attributes: [.font: UIFont.boldSystemFont(ofSize: UIFont.systemFontSize)]
            )
            attributedString.append(attrs)
        }
        
        // Colored text interpolation
        mutating func appendInterpolation(color: UIColor, text: String) {
            let attrs = NSAttributedString(
                string: text,
                attributes: [.foregroundColor: color]
            )
            attributedString.append(attrs)
        }
    }
}

// Usage Examples
let name = "John"
let score = 95

// Create styled text using natural string interpolation syntax
let message: StyledText = """
Welcome \(bold: name)!
Your score: \(color: .systemGreen, text: "\(score)")
"""

// Use in UIKit
let label = UILabel()
label.attributedText = message.attributedString
```

이 코드를 실행하면 볼드체와 초록색으로 강조된 텍스트가 화면에 표시된다.
![Attributed Strings](/assets/img/2025/01/06/image1.png)

## 정리
Swift 5에서 크게 개선된 문자열 보간 기능은 단순한 문자열 조합 이상의 가능성을 제공한다. 타입 안전성을 보장하면서도 우리가 원하는 형태로 자유롭게 확장할 수 있어, 다양한 상황에서 유연하게 활용할 수 있다.

특히 커스텀 타입과 결합했을 때 그 진가가 드러나는데, 복잡한 문자열 처리 로직을 깔끔하고 재사용 가능한 형태로 만들 수 있다는 점이 매력적이다.

<br>
**참고**
<br>

[Super-powered string interpolation in Swift 5.0](https://www.hackingwithswift.com/articles/178/super-powered-string-interpolation-in-swift-5-0){:target="_blank"}

[How to use custom string interpolation](https://www.hackingwithswift.com/articles/163/how-to-use-custom-string-interpolation-in-swift){:target="_blank"}

[SE-0228](https://github.com/swiftlang/swift-evolution/blob/main/proposals/0228-fix-expressiblebystringinterpolation.md){:target="_blank"}

[The Beauty of Swift 5 String Interpolation](https://ericasadun.com/2018/12/12/the-beauty-of-swift-5-string-interpolation/){:target="_blank"}