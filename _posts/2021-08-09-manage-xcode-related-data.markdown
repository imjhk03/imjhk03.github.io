---
title: Xcode 저장공간 이해 및 관리
layout: post
tags: xcode
---

> 해당 글은 [Understanding and Managing Xcode Space](https://www.raywenderlich.com/19998365-understanding-and-managing-xcode-space) 글을 보고 정리한 글입니다.

간혹 Xcode 빌드할 때 문제가 생기는데, 많이 사용해본 해결 방법은 바로 Derived Data 폴더를 지우는 것이다. Derived Data 폴더가 생각보다 저장공간을 많이 차지하는데, 삭제하면 어느 정도 저장공간이 확보된다. 그리고 새로운 아이폰을 발표할 때마다 사용하지 못하는 시뮬레이터를 지우는 일을 매년 하고 있다. 이 부분도 mac 저장공간에 저장되고 있기 때문에 관련 파일들을 지우면 저장공간을 확보할 수 있다. 관련하여 검색하다가 Raywenderlich에서 잘 정리한 글이 있어, 해당 글을 보고 정리를 해봤다.

## Xcode Derived Data

프로젝트를 빌드할 때, Xcode가 해당 프로젝트의 빌드 파일들을 derived data 폴더에 저장한다. 위치는 아래 경로에 있다.

```zsh
~/Library/Developer/Xcode/DerivedData
```

![Derived Data 경로에 있는 폴더](/assets/img/2021/08/09/image1.png)

**ModuleCache.noindex** 폴더는 Xcode가 컴파일한 모듈(modules)을 저장한다. Xcode는 캐시된 모듈을 프로젝트들과 공유하며 빠르게 빌드할 수 있게 한다. 프로젝트 빌드 폴더도 동일하게 빠르게 빌드할 수 있게 해준다. 해당 프로젝트 폴더를 지우면 mac 저장 공간도 줄이고 이상한 버그도 해결할 수 있지만, 다음 빌드할 때 시간이 더 소요된다.

## Clearing Archives

프로젝트들을 archive하면 mac 어딘가에 저장이 된다. 몇 년간 archive하고 지우지 않는다면, 저장 공간을 꽤 많이 차지할 수 있다. 아카이브하면 아래 경로에 날짜별로 아카이브한 폴더가 생긴다. 날짜별 폴더에 들어가면 아카이브한 파일이 있다.

```zsh
~/Library/Developer/Xcode/Archives
```

![Archive한 폴더가 날짜별로 있다.](/assets/img/2021/08/09/image2.png)

날짜별 폴더에 들어가 보면 아래와 같이 아카이브 파일이 있는데, 해당 파일을 삭제하면 된다. 프로젝트 빌드에 영향을 끼치지 않고 저장 공간을 줄일 수 있지만, 혹시 다시 테스트플라이트 올려야 하는 경우를 위해서 지우지 않아도 괜찮다. 또한, 현재 서비스하고 있는 앱을 디버깅하는 데 필요한 파일인 dSYM 파일이 있는데, 아카이브 패키지 안에 있다. 되도록 더 사용하지 않을 것으로 판단할 때 지우는 것을 추천한다.

![프로젝트를 아카이브한 파일](/assets/img/2021/08/09/image3.png)

## Clearing Simulators

시뮬레이터에 빌드하게 되면 앱을 설치하고 실행해서 테스트할 수 있다. 사진을 저장하는 게 있다면, 시뮬레이터의 사진 앨범에 저장하게 된다. 하지만 설치한 앱을 삭제해도, 사진 앨범에 저장한 사진 등 기타 자료에 대해서는 남아 있을 수 있다. 완전하게 깨끗한 시뮬레이터로 테스트하고 싶다면 시뮬레이터의 콘텐츠를 지우는 방법이 있다.

시뮬레이터 킨 상태에서 **Device > Erase All Content and Settings** 메뉴를 선택하면 아래와 같이 시뮬레이터에 팝업이 나타난다. 지우고 싶다면 **Erase**를 누르면 된다. 이렇게 되면 저장했던 사진들이 지워지고 기타 Core Database 등을 지우게 된다.

![Xcode 시뮬레이터의 콘텐츠를 지우는 팝업이 뜬 화면](/assets/img/2021/08/09/image4.png)

## Deleting Unavailable Simulators

매년 새로운 아이폰 혹은 아이패드 기기를 출시하면 새로운 버전의 Xcode에서 해당 기기들이 포함해서 출시된다. 이렇게 하다 보면 오래돼서 안 쓰게 되는 혹은 못 쓰게 되는 시뮬레이터들이 생긴다. 이런 시뮬레이터들도 저장 공간을 차지한다.

아래 터미널에 명령어를 치면 사용하지 못하는 시뮬레이터들을 지울 수 있다.

```zsh
xcrun simctl delete unavailable
```

만약 사용하지 못하는 시뮬레이터들이 있었다면, 명령어를 친 후에 output으로 어떤 시뮬레이터들이 지웠는지 안내해준다. 만약 output이 없다면 사용하지 못하는 시뮬레이터들이 없다는 뜻이다.

## Device Support

기기를 맥이랑 연결해서 설치하거나 디버깅한다면, Xcode는 device support 파일들을 만든다. Xcode는 이렇게 생성된 파일들을 크래시 로그 보는 것과 같이 개발 관련 기능들을 지원할 때 사용한다. OS 버전 별로 만들며 마이너 버전까지 지원한다. 예를 들면 iOS 14.1, 14.2, 14.2.1 등.

Xcode는 해당 파일들을 제거하지 않아 시간 지날수록 계속 쌓일 수 있다. 기기를 연결할 때마다 Xcode가 파일들을 자동으로 생성하기 때문에 해당 파일들을 제거해도 괜찮다.

아래 경로에 iOS device support 파일들이 있다.

```zsh
~/Library/Developer/Xcode/iOS DeviceSupport
```

생각보다 저장 공간을 많이 차지하기 때문에 지원하는 iOS 버전을 몇 개 빼고 지우는 것을 추천한다. 해당 폴더는 iOS device만 있기 때문에 watchOS와 tvOS 따로 있으며, 해당 폴더도 정리해도 괜찮다.

![Xcode 시뮬레이터가 iOS 기기 서포트하는 폴더가 버전별로 있다.](/assets/img/2021/08/09/image5.png)

```zsh
~/Library/Developer/Xcode/watchOS DeviceSupport
~/Library/Developer/Xcode/tvOS DeviceSupport
```

## Caches

캐시는 캐시를 사용하는 프로그램을 빠르게 실행할 수 있도록 데이터를 저장한다. 캐시가 저장하는 데이터는 일시적으로 사용하는 데이터이다. 캐시는 프로그램이 실행할 때마다 생성되기 때문에, 지워도 괜찮다. 다만 대용량 캐시일 경우, 캐시가 재빌드하면서 시간이 좀 소요되는 것을 경험할 수 있다.

캐시 삭제는 공간을 재확보하기 위한 일반적인 전략이다. 예를 들면, Xcode 캐시를 삭제하면 오래되고, 사용하지 않는 데이터를 삭제한 채 남게 된다. 나중에 Xcode는 필요할 때 재생성해서 사용하게 된다.

가끔 Xcode 사용하면서 문제가 생긴다면 캐시를 지우는 것도 해결 방법의 하나다. 캐시는 보통 *~/Library/Caches* 폴더에 저장된다. Xcode 캐시도 동일하게 저장되는데, *~/Library/Caches/com.apple.dt.Xcode* 경로에 있다.

외부 라이브러리를 사용한다면 Carthage 혹은 CocoaPods를 사용하게 되는데, 이와 관련한 캐시들도 저장이 된다. Carthage 캐시의 경로는 *~/Library/Caches/org.carthage.CarthageKit* 이다. CocoaPods는 다르게 특별한 명령어를 이용해서 캐시를 삭제할 수 있다. 아래 명령어를 터미널에 치면 CocoaPods 캐시를 삭제한다.

```zsh
pod cache clean --all
```

## Conclusion

위의 내용을 정리하면서 Xcode 사용하면서 다양하게 빌드 및 프로젝트 관련된 파일들이 저장되는 것을 알게 되었고, 관련 파일들은 어디에 저장되는지 그리고 삭제 가능한지도 알아볼 수 있었다. 덕분에 mac 저장공간을 효율적으로 관리할 수 있는 팁을 알게 되었다.