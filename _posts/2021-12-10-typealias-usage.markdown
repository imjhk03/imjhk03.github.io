---
title: 스위프트 typealias 활용하기
layout: post
tags: [swift]
---

스위프트에 있는 `typealias`는 기존에 존재하는 타입을 다른 이름으로 정의할 때 사용합니다. 상황에 따라서 기존에 있는 타입을 조금 더 적합한 이름으로 사용할 때 유용합니다. 예를 들어, 가격이 Int 타입인 상품 구조체struct가 있습니다.

```swift
struct Product {
    let price: Int
}
```

`Int` 타입으로 사용해도 괜찮으나, 화폐 단위로 사용하고 싶다면 `typealias`를 이용해서 Won이라는 별칭을 만들어 사용할 수 있습니다.

```swift
typealias Won = Int

struct Product {
    let price: Won
    let shippingCost = Won.zero
}
```

이와 같이 기존에 존재하는 타입을 별칭을 만들어서 사용할 수 있습니다. 하지만 typealias를 다른 용도로 활용할 수 있는데 몇 가지를 소개하겠습니다.

## 외부 라이브러리 타입 구분

외부 라이브러리를 사용하면 라이브러리 내에서 만든 클래스 혹은 타입들이 보통 특이한 이름을 가지거나 구분할 수 있는 접두사가 붙여진 이름을 사용합니다. 하지만 간혹 외부 라이브러리에서 만든거라고 구분하기 힘든 이름들이 있습니다. 예를 들어, [Lottie](https://github.com/airbnb/lottie-ios)라는 애니메이션 관련 라이브러리에서 애니메이션을 다루는 view인 `AnimationView`가 있습니다. 만약 프로젝트 안에서 이미 `AnimationView`라는 커스텀 view를 이미 사용하고 있다면 구분하기 어려울 수 있습니다. 이럴 때, 외부 라이브러리에서 만든 view로 인식할 수 있도록 typealias를 이용해서 `LottieAnimationView`라고 별칭을 만들어 사용할 수 있습니다.

```swift
typealias LottieAnimiationView = AnimationView

private var confirmAnimationView: LottieAnimationView?
```

## Tuple의 이름으로 만들어 사용하기

스위프트에 있는 튜플은 다수의 값들을 하나의 구성된 값으로 사용할 수 있게 해줍니다. 튜플을 하나의 이름으로 만들어 사용할 수 있는데, typealias를 이용해서 사용할 수 있습니다.

```swift
typealias SaleRange = (min: Int, max: Int)

let saleRange = SaleRange(min: 0, max: 100)
let maxSaleRange = saleRange.max
```

## 다수의 프로토콜 채택하기

다수의 프로토콜을 채택하여 구현해야 하는 경우가 있는데, 이럴 때 typealias를 이용해서 조합해서 만들 수 있습니다. 아마 가장 대표적인 것은 Codable이 있습니다.

```swift
typealias Codable = Decodable & Encodable
```

이와 비슷하게 다수의 프로토콜을 채택하여 구현해야 하는 경우가 있는데, 바로 tableview 혹은 collectionview와 delegate 패턴을 사용하는 경우가 있습니다. 예를 들어 다양한 cell로 구성된 collectionview가 있는 viewcontroller가 있습니다. 각 cell에서 어떤 동작을 취하면 viewcontroller가 처리하도록 각 cell의 프로토콜을 만들어 delegate을 이용해서 넘길 수 있습니다. 이럴 때, 하나의 `Delegate`라는 별칭을 만들어서 다수의 프로토콜을 채택하도록 만들 수 있습니다.

먼저, collectionview의 DataSource와 Delegate를 담당할 CollectionViewDataSource를 하나 생성하고, cell을 configure할 때, delegate를 설정하도록 합니다.

```swift
class CollectionViewDataSource: NSObject, UICollectionViewDelegate, UICollectionViewDataSource, UICollectionViewDelegateFlowLayout {

    typealias Delegates = ProductCellDelegate & ProductHeaderDelegate

    private weak var delegates: Delegates?

    init(delegates: Delegates?) {
        self.delegates = delegates
        // do additional work if needed
    }

    // cellForItemAt에서 cell 만들고 delegate를 할당한다
    func collectionView(_ collectionView: UICollectionView, cellForItemAt indexPath: IndexPath) -> UICollectionViewCell {
        let cell = collectionView.dequeueReusableCell(withReuseIdentifier: "Cell", for: indexPath) as! ProductCell
        
        guard let product = products[indexPath.item] else { return cell }
        cell.configure(product: product, delegate: delegates)
        return cell
    }

    // viewForSupplementaryElementOfKind에서 header 만들고 delegate를 할당한다
    func collectionView(_ collectionView: UICollectionView, viewForSupplementaryElementOfKind kind: String, at indexPath: IndexPath) -> UICollectionReusableView {
        let section = indexPath.section
        switch kind {
            case UICollectionView.elementKindSectionHeader:
                let header = collectionView.dequeueReusableSupplementaryView(ofKind: kind, withReuseIdentifier: "Header", for: indexPath) as! ProductHeaderView
                header.delegate = delegate
                return header
            default:
                assert(false, "Invalid element type")
        }
    }
}
```

그다음에는 viewcontroller에서 CollectionViewDataSource를 가지고 collectionview 구성할 때, viewcontroller가 프로토콜을 채택하여 구현하도록 하고 CollectionViewDataSource의 delegates를 viewcontroller로 할당하면 됩니다.

```swift
class ViewController: UIViewController {

    @IBOutlet weak var collectionView: UICollectionView!

    ...
    
    func setupCollectionView() {
        let dataSource = CollectionViewDataSource(delegates: self)
        collectionView.dataSource = dataSource
        collectionView.delegate = dataSource
    }

}

extension ViewController: ProductCellDelegate {
    // Implement ProductCellDelegate
}

extension ViewController: ProductHeaderDelegate {
    // Implement ProductHeaderDelegate
}
```

## Closure typealias 사용하기

클로저를 typealias를 이용해서 축약하여 타입 만들어 사용할 수 있습니다. 반복적인 closure가 있을 경우 유용하게 사용할 수 있습니다.

```swift
typealias CompletionHandler<T> = (Result<T, NetworkError>) -> Void

func fetchProducts(id: String, then handler: @escaping CompletionHandler<Product>) {
    ...
}

func fetchStores(id: String, then handler: @escaping CompletionHandler<Store>) {
    ...
}
```

## 마무리

스위프트의 typealias를 기존의 타입을 다른 이름으로 사용할 수 있지만, 코드를 조금 더 깔끔하게 하거나 읽기 쉬워지는 효과를 볼 수 있습니다. Typealias를 활용해서 프로젝트의 코드를 조금 더 개선하거나 읽기 쉬운 코드로 바꾸는 작업을 해보면 좋을 것 같습니다.

스위프트 관련 글을 더 보고 싶다면 [스위프트 카테고리 페이지](https://imjhk03.github.io/tags/swift/)를 확인해 볼 수 있으며, 질문이나 피드백이 있다면 [트위터](https://twitter.com/_jooheekim_) 혹은 <a href="mailto:imjhk03@gmail.com">이메일</a>로 연락 주세요.

글 읽어주셔서 감사합니다.