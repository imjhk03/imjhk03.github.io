---
title: 스위프트에서 빌더 패턴 구현해보기
layout: post
tags: [architecture, design pattern]
---

최근에 회사에서 커스텀 해서 사용하는 뷰를 사용해야 하는 경우가 생겼는데, 정해진 규칙이 있고 다양하게 조립하여 뷰를 그릴 수 있다고 판단하여 builder pattern(이하 빌더 패턴)으로 만들어 구현해 보았다. 회사 직원들도 잘 만들었다고 했고, 직접 사용하면서 불편한 점이 없다고 해서 다행이라고 생각했다. 다른 부분에도 적용하여 만들어보고 싶어, 혼자 공부했던 것을 정리할 겸 글을 쓰게 되었다. 빌더 패턴이 무엇인지, 어떻게 만들어 사용하면 좋을지 소개하겠다.

## 객체를 생성하다

빌더 패턴이란 객체를 생성할 때 사용하는 패턴으로, 자바에서 많이 사용한다. 복잡하게 많은 파라미터를 넣어 객체를 생성하지 않고, 차례대로 값을 넣으면서 객체를 생성하는 식의 구조다. 예를 들면, 아래와 같이 카드 형태의 뷰를 만든다고 해보자. 카드 형태의 뷰를 그리기 위해 타이틀과 모서리 반경, 색깔을 설정한다.

```swift
let cardView = CardView()
cardView.titleLabel.text = "New app update"
cardView.contentView.cornerRadius = 8
cardView.contentView.backgroundColor = .systemBlue
```

카드 뷰를 직접 생성해서, 그리는데 필요한 부분들에 직접 값을 할당해서 사용하고 있다. 만약 뷰의 서브 뷰를 직접 접근하지 않고 싶다면, configure 함수를 만들어서 사용할 수 있다. 이제 여기서 서브타이틀이 필요하고 이미지가 필요하다면, 각 서브 뷰들을 추가하고 configure 함수에 파라미터를 추가해서 설정해야 한다. 점점 서브 뷰들이 많아지면, configure 함수의 파라미터도 많아진다. 기본값이 있다면, swift의 default value를 이용해서 불필요한 설정을 줄일 수 있다. 하지만 파라미터가 많아지는, 즉 설정하는 부분이 점점 복잡해지고 더러워지는 문제를 피할 수 없다. 빌더 패턴이 이 문제를 해결할 수 있다.

```swift
let cardView = CardViewBuilder()
	.withTitle("New app update")
	.withCornerRadius(8)
	.withBackgroundColor(.systemBlue)
	.build()
```

빌더 패턴을 사용하면 복잡한 생성자를 줄일 수 있고, 위와 같이 프로퍼티에 접근해서 사용하는 부분을 private으로 하여 외부에서 접근 못 하게 할 수 있다. 빌더 패턴으로 자기 자신을 return 하여 configure 하는데, 이 부분은 chaining 하는 것 처럼 사용하여, 지금의 SwiftUI와 매우 비슷한 것을 볼 수 있다.

```swift
Text("This is title")
    .foregroundColor(.red)
    .font(.system(size: 14, weight: .bold, design: .default))
```

## 빌더 패턴 구조

빌더 패턴에 대해서 더 자세히 보면, 구조는 Director, Builder, Product으로 구성되어 있다.

- Product - 생성하고 싶은 객체
- Builder - 생성하고 싶은 Product를 생성해 주고 반환해 주는 구성 요소
- Director - Builder를 이용해서 필요한 product를 받아서 처리하는 구성 요소. 사용하는 용도에 따라 Director를 이용할 수 있고, 직접 Builder를 접근해서 product 반환할 수 있다.

![빌더 패턴의 구조를 설명한 간단하게 그린 클래스 패턴](/assets/img/2021/09/23/image1.jpeg)

## 구현하기

간단하게 Builder를 하나 만들어서 구현할 수 있다. 커스텀 해서 사용하는 뷰를 그리는 빌더를 만들 수 있고, 상품 혹은 자동차와 같이 객체를 만드는 빌더를 만들 수 있다. Director를 사용하여 빌드 패턴을 만들어보기로 하고, 피자를 만드는 빌드 패턴을 구현할 것이다.

> 빌더 패턴에 맞게 구현한 것이기 때문에 클래스가 많고 불필요한 코드가 보일 수 있습니다. 사용하려는 프로젝트에 맞춰서 구현해서 만들어 사용하는 것을 권장합니다.

### Product

피자는 도우와 토핑, 사이즈를 구성하는 클래스를 만든다.

```swift
 class Pizza {
    private var dough: Dough = .basic
    private var toppings = [Topping]()
    private var size: Size = .regular

    func setDough(_ dough: Dough) {
        self.dough = dough
    }

    func setToppings(_ toppings: [Topping]) {
        self.toppings = toppings
    }

    func setSize(_ size: Size) {
        self.size = size
    }

    func listPizzaInfo() -> String {
        let toppingList = toppings.map { $0.rawValue }
        return "Your pizza's size is \(size.rawValue) and the dough is \(dough.rawValue), toppings are \(toppingList.joined(separator: ", "))."
    }
}
```

### Builder

피자들은 공통적으로 사이즈에 맞게, 도우를 선택하고, 토핑을 뿌린 다음에 만들어진다. 프로토콜을 만들고, 빌더가 채택해서 구현하도록 만든다.

```swift
protocol Builder {
    func reset()
    func setDough(_ dough: Dough)
    func addToppings(_ toppings: [Topping])
    func setSize(_ size: Size)
}

class PizzaBuilder: Builder {
    private var pizza = Pizza()

    func reset() {
        self.pizza = Pizza()
    }

    func setDough(_ dough: Dough) {
        self.pizza.setDough(dough)
    }

    func addToppings(_ toppings: [Topping]) {
        self.pizza.setToppings(toppings)
    }

    func setSize(_ size: Size) {
        self.pizza.setSize(size)
    }

    func getPizza() -> Pizza {
        let result = self.pizza
        reset()
        return result
    }

}
```

피자를 구성하는 요소들을 설정하는 부분들이 주로 있으며, 여기서 getPizza 함수를 이용해서 만든 pizza 프로덕트를 반환한다. reset 함수가 있는 이유는 만들어진 프로덕트를 반환하고 새로운 프로덕트를 만들기 위해서 초기화하는 작업으로 보면 된다. 한 빌더에 많은 프로덕트를 만들 수 있기 때문에 새로운 프로덕트를 만들기 위한 준비로 보면 된다. 하지만 일회성으로 빌더 만들고 사용한다면, 굳이 reset 함수가 필요 없을 수 있다.

### Director

피자 빌더를 이용해서 피자를 만들고 사용할 director 클래스는 꼭 필요한 요소는 아니다. View Controller 혹은 빌더를 사용하는 부분에서 직접 빌더를 만들어서 사용할 수 있기 때문이다. 여기서는 간단하게 치즈 피자와 페퍼로니 피자를 만드는 Director 클래스를 만들었다.

```swift
class Director {
    private var builder: Builder?
    
    func buildCheesePizza(_ builder: Builder) {
        builder.reset()
        builder.setDough(.cheeseCrust)
        builder.addToppings([.extraCheese, .mushrooms, .sausage, .greenPeppers, .blackOlives])
        builder.setSize(.regular)
    }
    
    func buildPepperoniPizza(_ builder: Builder) {
        builder.reset()
        builder.setDough(.basic)
        builder.addToppings([.pepperoni, .mushrooms, .sausage, .greenPeppers, .blackOlives])
        builder.setSize(.regular)
    }
}
```

### Client

실제로 프로덕트인 피자가 필요한 클라이언트 부분이 있다. Director를 통해서 피자를 얻어서 사용한다. Director와 마찬가지로 꼭 필요한 구성 요소는 아니다. 직접 빌더를 만들어서 사용할 수 있기 때문이다.

```swift
class Client {
    func makeCheesePizza() {
        let director = Director()
        let pizzaBuilder = PizzaBuilder()
        
        director.buildCheesePizza(pizzaBuilder)
        let cheesePizza = pizzaBuilder.getPizza()
        
        print(cheesePizza.listPizzaInfo())
    }
    
    func makePepperoniPizza() {
        let director = Director()
        let pizzaBuilder = PizzaBuilder()
        
        director.buildPepperoniPizza(pizzaBuilder)
        let pepperoniPizza = pizzaBuilder.getPizza()
        
        print(pepperoniPizza.listPizzaInfo())
    }
}

let client = Client()
client.makeCheesePizza()
print()
client.makePepperoniPizza()

// Prints
// "Your pizza's size is regular and the dough is cheeseCrust, toppings are extraCheese, mushrooms, sausage, greenPeppers, blackOlives."
// "Your pizza's size is large and the dough is basic, toppings are pepperoni, mushrooms, onions, blackOlives."
```

코드가 생각보다 굉장히 길고 클래스가 많아진 것을 볼 수 있다. 하지만 실제로 client에서 호출해서 사용하는 부분을 보면 단순하다고 볼 수 있다. 구현하는 부분은 빌더가 하고, 클라이언트는 빌더가 만든 피자를 받아서 사용하면 된다. 완성된 전체적인 코드는 [gist](https://gist.github.com/imjhk03/188cdf0ac899cf767b2ec71bf7ec3399)에서 확인할 수 있다.

작업하는 프로젝트 성격에 맞춰서 조금 더 가볍게 구현할 수 있다. 위에서 예를 든 카드 뷰처럼 직접 빌더를 만들고 설정하는 부분을 클라이언트 단(view controlller일 수 있고, 다른 부분일 수 있는)에서 직접 호출해서 써도 된다. 맨 처음 만든 예시처럼 pizza builder를 만든다면, 아래와 같을 수 있다.

```swift
let pizza = PizzaBuilder()
	.withSize(.regular)
	.withToppings([.extraCheese, .mushrooms, .sausage, .greenPeppers, .blackOlives])
	.withDough(.cheeseCrust)
	.build()

print(pizza.listPizzaInfo())
// Prints "Your pizza's size is regular and the dough is cheeseCrust, toppings are extraCheese, mushrooms, sausage, greenPeppers, blackOlives."
```

## UIStackView와 사용하기

빌더 패턴과 잘 어울려 사용할 수 있는 컴포넌트는 개인적으로 `UIStackView`라고 생각한다. 차례대로 필요한 부분들을 만드는데, 서브 뷰들을 하나씩 만들어 `addArrangedSubview(_:)`를 이용해서 추가할 수 있다. 또한, 순서대로 만들고 싶다면, 빌더 패턴의 빌드하는 부분의 순서를 변경해서 `UIStackView`에 순서대로 추가할 수 있다.

커스텀 뷰가 프로덕트에 해당하기 때문에, 해당 프로덕트에 stack view를 만들어서 서브 뷰들을 생성해서 stack view에 추가하는 식으로 구현할 수 있다.

```swift
class CustomView: UIView {

    private let containerView = CustomContainerView()

    private func configureContainerView() {
        // configure basic settings
    }

    func addTitle(_ title: String, spacing: CGFloat = 10) {
        let label = UILabel()
        ...
        containerView.stackView.addArrangedSubview(label)
    }

    func withSubTitle(_ subTitle: String) {
        let label = UILabel()
        ...
        containerView.stackView.addArrangedSubview(label)
    }

    func addImage(_ image: UIImage, size: CGSize) {
        let imageView = UIImageView(image: image)
        ...
        containerView.stackView.addArrangedSubview(imageView)
    }

    func addButtons(_ buttons: [CustomButton]) {
        containerView.addButtonStackView()
        buttons.forEach {
            containerView.buttonStackView.addArrangedSubview($0)
        }
    }

}
```

```swift
let customView1 = CustomViewBuilder()
	.withTitle("Notice")
	.withSubTitle("Are you sure to delete this list?")
	.withButtons([.cancelButton, .okButton])
	.build()

let customView2 = CustomViewBuilder()
	.withImage(defaultImage)
	.withButtons([.cancelButton, .okButton])
	.withTitle("Notice")
	.withSubTitle("Are you sure to delete this list?")
	.build()
```

순서가 영향이 끼칠 수 있는 뷰를 그릴 때, 빌더 패턴을 이용하고 ```UIStackView```를 이용한다면 쉽게 구현해서 사용하는 것을 볼 수 있다.

## 주의점

빌더 패턴은 수많은 구성 요소를 설정해야 하는 객체를 생성할 때 사용하면 좋다. 불필요하게 코드가 늘어날 수 있기 때문에, 실제로 객체를 만들 때 구성 요소가 많은지, 생성자 하나로도 충분한지 고려해서 빌더 패턴을 사용할지 결정해야 한다. 아니면, 위에서 예시로 든 stack view를 이용해서 화면을 그리는 부분이 필요하다면 사용할 수 있다. 물론 프로젝트의 크기에 따라 적용할지는 고려해야 한다.

## 결론

빌더 패턴은 복잡한 객체를 생성할 때 차례대로 만들어 사용할 수 있을 때 효과적이다. 스위프트에서는 빌더 패턴이 흔하지 않는 패턴이지만, 필요하다면 충분히 빌더 패턴을 만들어서 적용할 수 있는 것을 볼 수 있다. 작업하는 프로젝트에 빌더 패턴으로 만들어 사용할 수 있는 부분이 있다면 적용해 보는 것을 추천드립니다.

<br>

gist: [PizzaBuilder.swift](https://gist.github.com/imjhk03/188cdf0ac899cf767b2ec71bf7ec3399)

참고: [Builder](https://refactoring.guru/design-patterns/builder)