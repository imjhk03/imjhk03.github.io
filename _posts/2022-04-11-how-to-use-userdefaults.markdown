---
title: UserDefaults를 사용하는 방법
layout: post
tags: [userdefaults, foundation]
---

iOS에서는 사용자 설정 같이 정보량이 적은 데이터들을 앱이 설치되어 있는 동안 저장하여 사용할 수 있습니다. 바로 **`UserDefaults`**를 사용해서 저장하는 방법입니다. **`UserDefaults`**는 integer, boolean, string, array, dictionary, URL 같은 타입들을 저장할 수 있습니다.

```swift
let defaults = UserDefaults.standard
defaults.set(30, forKey: "Age")
defaults.set("JooHee", forKey: "Name")
defaults.set(false, forKey: "NotificationAllowed")

let array = ["Apple", "Banana"]
defaults.set(array, forKey: "Fruits")

let dict = ["Name": "Joohee", "Country": "KOR"]
defaults.set(dict, forKey: "UserInfoDict")
```

<br>
이와 같이 데이터들을 저장하면 영구적으로 저장이 됩니다. 즉, 앱을 다시 켰을 때도 저장했을 때와 똑같은 정보가 저장되어 있습니다. 저장한 데이터들을 다시 읽을 때도 쉽게 할 수 있습니다.

```swift
let age = defaults.integer(forKey: "Age")
let isNotificationAllowed = defaults.bool(forKey: "NotificationAllowed")
let name = defaults.string(forKey: "Name")
```

<br>
다만, 만약 저장되어 있는 데이터가 없다면 **`UserDefaults`**에서는 default value를 리턴합니다. 실제로 저장한 데이터와 기본값을 구분하기 위해서 어떤 기본값을 리턴하는지 유의해서 사용해야 합니다.

- **`integer(forKey:)`**: returns 0
- **`double(forKey:)`**: returns 0
- **`bool(forKey:)`*** returns false
- **`string(forKey:)`**: returns `nil`
- **`object(forKey: String)`**: returns `Any?`

<br>
위 string과 object 경우에는 옵셔널한 값이 리턴됩니다. 옵셔널 값을 받으면 원하는 데이터 타입으로 타입 변환해서 사용해야 합니다. 비어 있는 값이면 기본적으로 사용할 값을 ?? 연산자(nil coalescing operator)를 이용해서 지정합니다.

```swift
let savedFruits = defaults.object(forKey: "Fruits") as? [String] ?? [String]()
```

<br>
**`UserDefaults`**에 저장된 값을 지우고 싶을 때는 **`removeObject(forKey:)`** 메소드를 사용합니다.

```swift
defaults.removeObject(forKey: "Age")
```

<br>
**`UserDefaults`**는 싱글톤이고 thread-safe입니다.

<br>
**참고**

[https://developer.apple.com/documentation/foundation/userdefaults](https://developer.apple.com/documentation/foundation/userdefaults)