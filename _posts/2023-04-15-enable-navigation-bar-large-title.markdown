---
title: 네비게이션 바에서 큰 타이틀 사용하기
layout: post
tags: [UIKit]
---

아이폰 기본 앱들 중에 설정 앱이나 노트 앱, 미리 알림 앱 등에서는 화면의 타이틀이 크게 나타나는 화면을 볼 수 있다. iOS 11부터 소개가 되었는데, 처음 나타는 화면에서는 타이틀을 크게 보여주고 스크롤을 하면 가운데 중앙 정렬 작게 나타나는 네비게이션 바 디자인이다.

기본적으로 비활성화되어 있는데 아래 코드를 작성하면 큰 타이틀을 표시할 수 있다.

```swift
navigationController?.navigationBar.prefersLargeTitles = true
```

활성화가 되면 navigation controller에서 푸시 된 모든 view controller에 영향을 끼쳐 제목이 크게 나타나게 되는데, `navigationItem.largeTitleDisplayMode` 프로퍼티를 사용해서 조정할 수 있다.

이동이 된 view controller의 제목을 크게 적용하고 싶지 않다면, 해당 view controller의 viewDidLoad() 메서드 안에 아래 코드를 작성하면 된다.

```swift
navigationItem.largeTitleDisplayMode = .never
```

보통 가장 처음으로 진입하는 화면의 제목을 크게 설정하고 그다음으로 진입하는 화면의 제목을 크게 적용하지 않도록 하면 되는데, 특정 화면에 제목을 크게 보여주고 싶다면 조절하면 된다.

**참고**
<br>
[prefersLargeTitles | Apple Developer Documentation](https://developer.apple.com/documentation/uikit/uinavigationbar/2908999-preferslargetitles)
<br>
[largeTitleDisplayMode | Apple Developer Documentation](https://developer.apple.com/documentation/uikit/uinavigationitem/2909056-largetitledisplaymode)
