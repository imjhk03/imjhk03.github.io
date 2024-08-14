---
title: 화면 상태에 따라서 View가 그리는 작업, Render
layout: post
tags: [architecture, design patterns, view]
image:
  path: https://images.unsplash.com/photo-1453728013993-6d66e9c9123a?ixlib=rb-1.2.1&q=85&fm=jpg&crop=entropy&cs=srgb
  alt: Image from Unsplash
---

MVVM 패턴과 view와 view model을 바인딩 하는 부분을 rxswift를 사용하면서 편리함을 많이 느꼈습니다. 네트워크 호출해서 받은 데이터를 화면에 뿌릴 때, 혹은 화면에서 user interaction을 받았을 때 등의 처리를 간결하게 처리할 수 있는 것을 배웠습니다. 구독하는 개념이 다소 생소했고 아주 가볍게만 써보지만, 많이들 사용한다는 rxswift에 대해서 새로운 것을 배울 수 있어서 재밌었습니다. 가끔 rx를 바인딩 하는 것을 까먹을 때가 있었지만, 그 부분만 잘 기억하면 잘 활용할 수 있었습니다.

## Too much property

RxSwift를 잘 사용하다가, view와 view model을 양방향으로 바인딩 하기 위한 프로퍼티가 많아지면서 view controller가 비대해지는 것을 경험했었습니다. 회원가입 개편 작업하면서 사용자가 입력하는 값에 따라 textfield 화면이 달라지는 작업이 있습니다. 예를 들어 유효성 검사에 맞지 않거나 필수 값을 입력하지 않을 때 등, 빨간 테두리를 감싸면서 화면을 update 합니다. 혹은 휴대폰 인증 절차에 따른 팝업을 띄우거나 추가 절차가 생깁니다. 하지만 화면 혹은 데이터를 다뤄야 하고, 그것을 구독하기 위해 만든 프로퍼티들이 많다 보니, view controller에서 rx 바인딩 하는 부분이 너무 길어졌습니다.

아래 코드는 회원가입 view controller에서 rx 바인딩 하는 부분을 간결하게 표현한 것입니다. 회원가입 화면 특성상 많은 것을 다뤄야 하기 때문에 그럴 수 있지만, 조금 더 코드가 더 간결하면 어땠을까 아쉽게 작업한 부분입니다.

```swift
private func setupRx() {
    viewModel?.isIDDuplicate
        ...        
        
    viewModel?.emailResult
        ... 
        
    viewModel?.isVerifiedSuccess
        ... 
        
    viewModel?.userVerifyRequestResult
        ... 
        
    viewModel?.signUpResult
        ... 
        
    viewModel?.isLoading
        ...            
}
```

작업하면서 복잡한 화면일 경우 더 많은 프로퍼티를 구독할 수 있는 상황이 발생할 텐데, 그렇다면 view controller 코드도 길어지는 게 아닌가 생각이 들었습니다. MVC 패턴을 개선하고자 사용하는 MVVM 패턴이 점점 MVC의 Massive View Controller 특징을 나타내고 있어, 개선할 수 없는 부분이 없을까 자료를 찾아보았습니다.

## 화면의 상태, view state

앱 renewal 작업을 하기 위해 아키텍처 자료 찾는 시간에 저는 MVC 패턴에 대해서 찾아봤는데, MVC 패턴을 더 효율적으로 하는 방법 중 하나인 logic 담당하는 `logic controller`를 만들어 사용하는 글을 찾았습니다. MVC 패턴에서 비즈니스 로직을 `logic controller`에서 다루도록 작업하는 방법을 설명해 줬는데, 이와 함께 enum으로 view의 상태를 case 별로 정리해서 `ViewState`라는 것을 사용하고 있었습니다. 찜한 스토어 화면으로 예를 들어 설명하겠습니다.

```swift
enum BookmarkStoreState {
    case loading
    case presenting
    case failed
}
```

찜한 스토어 화면에서 어떤 이벤트가 발생하면, `BookmarkStoreViewController`가 `BookmarkStoreViewModel`에게 이벤트 핸들링하고 그에 따른 새로운 `BookmarkStoreState`를 return해서 view controller가 화면 그리는 작업을 합니다. 예를 들어, 찜한 스토어 view가 로드되는 시점에, view model에게 `loadMainData()`를 호출합니다. 그에 따른 화면 상태의 값을 다루는 `bookmarkStoreState` 프로퍼티는 rxswift 사용했습니다.

```swift
final class BookmarkStoreViewController: UIViewController {

    private let viewModel = BookmarkStoreViewModel()

    override func viewDidLoad() {
        super.viewDidLoad()
    
        ...

        setupRx()
        viewModel.loadMainData()
    }
}

private extension BookmarkStoreViewController {
    func setupRx() {
        viewModel.bookmarkStoreState
            .subscribe(onNext: { [weak self] state in
                self?.render(state)
            })
            .disposed(by: disposeBag)
    }
}
```

`BookmarkViewModel`에서는 `bookmarkStoreState` 프로퍼티에 화면의 상태 값을 할당해 줍니다. View controller는 ViewState 프로퍼티 때문에 네트워킹이 어떤 상태인지(`isFetching`, `isPaging` 등) 혹은 어떤 데이터가 변경되었는지 (`isVerifiedSuccess`, `isStatusUpdated` 등) 등을 알 필요가 없어집니다. 오로지 화면이 어떤 상태인지만 알면 되고, 거기에 따른 화면 작업만 하면 됩니다. 데이터 로딩하고 있다면 로딩 화면을, 데이터 로딩이 다 끝났으면 화면 그리는 작업을 하면 됩니다.

```swift
final class BookmarkStoreViewModel {
    
    let bookmarkStoreState = BehaviorRelay<BookmarkStoreViewController.ViewState>(value: .presenting)
    
    private let disposeBag = DisposeBag()

    func loadMainData() {
        fetchBookmarkMainData()
    }
    
    private func fetchBookmarkMainData() {
        NetworkService()
            .fetchItems()
            .do(onSuccess: { [weak self] _ in
                ...
            }, onSubscribed: { [weak self] in
                ...
                self?.bookmarkStoreState.accept(.loading)
            })
            .subscribe(onSuccess: { [weak self] result in
                guard let self = self else { return }
                
                switch result {
                case .success(let (responseModel, _)):
                    let data = responseModel.data
                    ...
                    self.bookmarkStoreState.accept(.presenting)
                case .failure(let error):
                    ...
                    self.bookmarkStoreState.accept(.failed)
                }
            })
            .disposed(by: disposeBag)
    }
}
```

만약 rxswift를 사용하지 않고, completion handler를 이용해서 view model에서 view state를 return 한다면, 아래와 같이 view model과 view controller의 코드가 작성됩니다.

```swift
// View Model
final class CategoryListViewModel {
    
    typealias CompletionHandler = (ViewState) -> Void
    
    enum ViewState {
        case loading
        case presenting
        case failed
    }

    func products(categoryId: String, then handler: @escaping CompletionHandler) {
        handler(.loading)

        ....

        Networking()
            .products(categoryId: categoryId, type: categoryType) { [weak self] result in
                guard let self = self else { return }
                switch result {
                case .success(let response):
                    ...
                    handler(.presenting)
                case .failure:
                    handler(.failed)
                }
            }
    }
}

// View Controller
final class CategoryListViewController: UIViewController {
    ...

    private func fetchProducts(categoryId: String) {
        ...
        
        viewModel?.products(categoryId: categoryId, then: { [weak self] state in
            self?.render(state)
        })
    }

}
```

## Renderer

View state 프로퍼티 값이 바뀌게 된다면, view controller에서 `render()` 메서드를 호출해서 화면 그림니다. 화면 상태에 따라서 case별로 분리해서 작업할 수 있습니다.

```swift
private extension BookmarkStoreViewController {
    func render(_ state: ViewState) {
        switch state {
        case .loading:
            // 로딩하는 화면 랜더
        case .presenting:
            // 응답받은 데이터를 가지고 화면 랜더
        case .failed:
            // 에러가 발생할 경우 화면 랜더
        }
    }
}
```

View controller에서 view model의 load 함수를 호출하면, view model에서는 load 함수에 따른 view state를 return 합니다. 그리고 view controller는 응답받은 view state를 가지고 화면 그리는 작업을 합니다. 흐름이 간단해졌고, view controller는 불필요한 프로퍼티에 대해서 알 필요 없어지고, 거기에 따른 화면 그리는 작업도 하지 않게 됩니다. 오로지 화면이 어떤 상태인지만 알고 거기에 따라서 화면을 그립니다.

## Conclusion

개인적으로 view controller의 코드 줄 수가 줄어들고, view controller가 view 작업만 하게 되는 것 같아  작업하면서 사용하고 있습니다. 이렇게 작업하는 부분에 대해서 여러분은 어떠신가요? 도움이 되었길 바라며 피드백은 환영입니다. 읽어주셔서 감사합니다.

<br>
**참고**
<br>

[Logic controllers in Swift \| Swift by Sundell](https://www.swiftbysundell.com/articles/logic-controllers-in-swift/)

간단한 코드를 보고 싶다면

[GitHub - imjhk03/BetterMVC: A project for better MVC pattern](https://github.com/imjhk03/BetterMVC)