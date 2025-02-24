---
title: Swift에서 여러 값을 한번에 switch하기
layout: post
categories: [Tips]
tags: [swift, switch statements, pattern matching]
---

스위프트(Swift)의 스위치(switch) 문은 패턴 매칭할 때 유용한데, 특히 여러 값을 동시에 매칭할 때 더욱 강력한 기능을 발휘한다. 튜플(tuple)을 사용하여 여러 값을 그룹화하고 한 번에 매칭할 수 있으며, 와일드카드(_)를 사용하여 특정 값을 무시할 수도 있다. 스위치문에서 케이스의 순서가 매우 중요하기 때문에 더 구체적인 케이스를 먼저 작성해야 한다.

다음은 뷰의 상태와 사용자 액션을 함께 처리하는 예제이다:

```swift
enum ViewState { case loading, presenting, failed }
enum UserAction { case pullToRefresh, tabRetry, tapClose }

func handle(state: ViewState, action: UserAction) {
    switch (state, action) {
    // 로딩 중에 당겨서 새로고침할 경우
    case (.loading, .pullToRefresh):
        showTopLoadingSpinner()
        
    // 로딩 중 다른 모든 액션
    case (.loading, _):
        showLoading()
        fetchNewData()
        
    // 컨텐츠 표시 중 당겨서 새로고침
    case (.presenting, .pullToRefresh):
        showTopLoadingSpinner()
        fetchNewData()
        
    // 실패 상태에서 재시도
    case (.failed, .tabRetry):
        showLoading()
        retryOperation()
        
    // 상태와 관계없이 닫기 처리
    case (_, .tapClose):
        dismissView()
        
    default:
        return
    }
}
```

`UICollectionView`의 `supplementary view`를 설정할 때도 여러 값을 switch하면 깔끔하게 처리할 수 있다:

```swift
func collectionView(_ collectionView: UICollectionView, 
                   viewForSupplementaryElementOfKind kind: String, 
                   at indexPath: IndexPath) -> UICollectionReusableView {
    let section = Section(rawValue: indexPath.section)
    
    switch (section, kind) {
    case (.popular, UICollectionView.elementKindSectionHeader):
        return configureView(at: .header, with: .popular)
    case (.popular, UICollectionView.elementKindSectionFooter):
        return configureView(at: .footer)
    default:
        return configureDefaultView()
    }
}
```