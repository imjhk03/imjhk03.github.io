---
title: iOS 15에서 Date.FormatStyle을 사용하여 Swift에서 날짜 서식 지정하기
description: iOS 15의 새로운 Date.FormatStyle API를 사용한 날짜 포맷팅 방법
layout: post
tags: [swift, dates]
---

Swift에서 날짜를 표시하고, 저장하고, 계산하는 작업을 자주 수행한다. iOS 15 이전에는 `DateFormatter`를 사용하여 이러한 작업을 처리해왔지만, 복잡하고 성능 비용이 높다는 단점이 있었다. iOS 15에서 Apple은 이러한 문제를 해결하기 위해 새로운 `Date.FormatStyle` API를 도입했다.

## 기존 DateFormatter의 한계점
먼저, 새로운 방식의 장점을 이해하기 위해 기존 `DateFormatter`의 한계점을 살펴보자:

```swift
// 기존 DateFormatter 방식
let dateFormatter = DateFormatter()
dateFormatter.dateStyle = .medium
dateFormatter.timeStyle = .short
dateFormatter.locale = Locale(identifier: "ko_KR")

let dateString = dateFormatter.string(from: Date())
// 2025년 4월 14일 오후 8:52
```

이 방식에는 몇 가지 단점이 있다:
* `DateFormatter` 인스턴스 생성 비용이 높다
* 여러 설정을 별도로 구성해야 한다
* 코드가 길어지고 가독성이 떨어진다
* 재사용을 위해 전역 인스턴스를 만들거나 캐싱 전략이 필요하다

## iOS 15의 새로운 접근 방식: Date.FormatStyle
iOS 15에서는 `formatted()` 메서드와 `Date.FormatStyle`을 통해 더 간결하고 선언적인 방식으로 날짜 포맷팅을 수행할 수 있다.

### 기본 사용법: formatted()
가장 간단한 형태로는 파라미터 없이 `formatted()`를 호출하는 것이다:

```swift
let date = Date()
print(date.formatted())
// 2025. 4. 14. 8:28 PM (사용자 로캘에 따라 다름)
```

이 메서드는 사용자의 현재 설정된 로캘과 지역 설정에 따라 자동으로 날짜를 포맷팅한다. 별도의 `DateFormatter` 인스턴스를 만들 필요가 없어 코드가 훨씬 간결해진다.

### 커스텀 포맷 스타일 지정하기
더 세밀한 제어가 필요한 경우, `Date.FormatStyle`을 사용하여 원하는 형식을 지정할 수 있다:
```swift
let date = Date()

let formattedDate = date.formatted(
    .dateTime
        .year()
        .month(.wide)
        .day(.twoDigits)
        .hour(.defaultDigits(amPM: .abbreviated))
        .minute(.twoDigits)
        .timeZone(.identifier(.long))
        .weekday(.abbreviated)
)
// Mon, April 14, 2025 8:45 PM Asia/Seoul
```

이 방식의 장점은 메서드 체이닝을 통해 원하는 요소만 선택적으로 포함할 수 있다는 점이다. 각 구성 요소에 대해 다양한 표시 옵션을 선택할 수 있어 높은 수준의 커스터마이징이 가능하다.

### 간결한 스타일 프리셋 사용하기
더 빠르게 날짜와 시간 형식을 지정하려면, 미리 정의된 스타일을 활용할 수 있다:
```swift
let date = Date()

// 날짜만 표시 (시간 생략)
print(date.formatted(date: .complete, time: .omitted))
// Monday, April 14, 2025

// 시간만 표시 (날짜 생략)
print(date.formatted(date: .omitted, time: .complete))
// 8:52:28 PM GMT+9

// 날짜와 시간 모두 간략하게 표시
print(date.formatted(date: .abbreviated, time: .shortened))
// April 14, 2025 at 8:52 PM
```

`Date.FormatStyle.DateStyle`과 `Date.FormatStyle.TimeStyle` 열거형은 다양한 프리셋을 제공한다:
* `.complete`: 모든 구성 요소를 포함한 완전한 형식
* `.long`: 상세한 형식
* `.abbreviated`: 축약된 형식
* `.numeric`: 숫자만 사용한 형식
* `.omitted`: 해당 부분 생략

### 로캘 지정하기
사용자의 현재 로캘이 아닌 특정 로캘로 날짜를 포맷팅하려면 다음과 같이 할 수 있다:
```swift
let date = Date()
let koreanStyle = Date.FormatStyle(locale: Locale(identifier: "ko_KR"))
let englishStyle = Date.FormatStyle(locale: Locale(identifier: "en_US"))

print(date.formatted(koreanStyle))
// 2025. 4. 14. 오후 8:52
print(date.formatted(englishStyle))
// 4/14/2025, 8:52 PM

let birthday = Date()
birthday.formatted(
	.dateTime
        .year()
        .month(.wide)
        .day(.twoDigits)
        .hour(.defaultDigits(amPM: .abbreviated))
        .minute(.twoDigits)
        .locale(Locale(identifier: "ko_KR"))	// 로캘 지정
)
// 2025년 4월 14일 오후 8:52
```

로캘을 명시적으로 지정하면 사용자의 기기 설정과 관계없이 일관된 형식으로 날짜를 표시할 수 있다.

## 결론
iOS 15의 `Date.FormatStyle`은 Swift에서 날짜 포맷팅을 더 간결하고, 선언적이며, 효율적으로 만들었다. 이 API를 사용하면 코드의 가독성이 향상되고, 유지 관리가 쉬워진다. iOS 15 이상을 지원하는 앱에서는 가능한 한 `Date.FormatStyle`을 활용하여 날짜 관련 기능을 구현하는 것이 좋다.

<br>
**참고**
<br>
[formatted\(\)](https://developer.apple.com/documentation/foundation/date/formatted%28%29){:target="_blank"}

[Date.FormatStyle](https://developer.apple.com/documentation/foundation/date/formatstyle){:target="_blank"}