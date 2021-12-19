---
title: Protocol extensions를 이용해서 기본값 제공하기
layout: post
tags: [swift, protocol]
---
프로토콜에 정의한 메서드는 기본값을 가질 수 없다. 하지만 extension을 이용해서 프로토콜에 정의한 메서드 혹은 프로퍼티에 기본값을 제공할 수 있다.

```swift
protocol Moveable {
    func move(to point: CGPoint)
}
```

위와 같이 Moveable 프로토콜에 메서드 하나를 정의했는데, 여기서 기본값을 지정한다면 Xcode에서 에러를 발생한다.

```swift
protocol Moveable {
    func move(to point: CGPoint = .zero)
    // Error: Default argument not permitted in a protocol method
}
```

여기서 extension을 이용해서 메서드에 기본값 제공하는 게 가능해진다.

```swift
extension Moveable {
    func move(to point: CGPoint = .zero) {
        move(to: point)
    }
}

struct Character: Moveable {
    let name: String
    
    init(name: String) {
        self.name = name
    }
    
    func move(to point: CGPoint) {
        print("Moving to \(point)")
    }
}

let mario = Character(name: "Mario")
mario.move(to: CGPoint(x: 3, y: 0))
let luigi = Character(name: "Luigi")
luigi.move()
```

여기서 주의점은 extension으로 프로토콜에 정의한 메서드를 구현했기 때문에, 프로토콜에서 채택하여 사용하는 부분에서 해당 메서드를 호출하면 extension에 있는 메서드를 부른다. 이렇게 되면 컴파일할 때는 문제가 되지 않으나, 앱을 실행할 때 재귀로 계속 자기 자신을 불러오는 EXC_BAD_ACCESS 에러가 발생한다. 프로토콜을 채택하는 부분에서 꼭 구현을 하거나 조금 더 안전한 방법으로는 기본값으로 설정하고 싶은 파라미터를 뺀 메서드를 만들어서 사용하면 된다.

```swift
extension Moveable {
    func move() {
        return move(to: .zero)
    }
}
```

프로퍼티에도 동일하게 기본값을 제공할 수 있다.

```swift
protocol Talkable {
    var greeting: String { get }
}

extension Talkable {
    var greeting: String {
        "Hello"
    }
}

struct Character: Moveable, Talkable {
    let name: String
    
    init(name: String) {
        self.name = name
    }
    
    func move(to point: CGPoint) {
        print("Moving to \(point)")
    }
}

let mario = Character(name: "Mario")
print(mario.greetings)
// Prints "Hello"
```