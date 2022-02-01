---
title: 'dyld: library not loaded error 해결하기'
layout: post
categories: Tips
tags: cocoaPods
---

최근에 프로젝트에 TDD를 위해서 오픈소스 라이브러리 추가하여 작업 진행 중에 있습니다. ```RxTest``` 및 기타 라이브러리 추가하여 작업한 부분을 로컬 리파지토리에 병합하려고 하니 build 에러가 떴습니다. 다른 작업자분께서 작업한 부분이라 pull 받고 나서 에러가 떴는데, 아래와 같이 Xcode 콘솔에 나타났습니다.

```
dyld: Library not loaded: @rpath/XCTest.framework/XCTest
...
Reason: image not found
```

위 에러를 해결하려고 굉장히 많은 시간을 허비했는데(~~한마디로 삽질~~), 생각보다 간단하게 해결할 수 있었습니다. 찾다보니 ```RxTest``` 등 테스트를 위한 오픈소스 라이브러리라면 테스트 타겟일때만 추가해줘야합니다.

```
target '{Test Target Name}' do
    inherit! :search_paths
    #Pods for testing
    
    pod 'RxTest', '5.1.1'
end
```

Podfile이 위와 같이 테스트 타겟에만 테스트 필요한 라이브러리 추가되었는지 확인하면 해당 에러는 해결됩니다.
