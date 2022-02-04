---
title: CocoaPods 팁 (설치부터 오류 해결)
layout: post
categories: Tips
tags: [cocoaPods]
---

앱 프로젝트 진행하다 보면, 오픈소스 라이브러리를 사용할 때가 있다. Xcode 프로젝트에 오픈소스 라이브러리를 설치 및 연결하는 방법이 CocoaPods, Carthage 또는 Swift Package Manager를 사용한다. 대표적으로 CocoaPods를 많이 사용하는데, CocoaPods 이용할 때 유용한 팁들을 정리해 보았다.

# CocoaPods 란?
> CocoaPods is a dependency manager for Swift and Objective-C Cocoa projects.

스위프트와 오브젝티브-C 코코아 프로젝트를 위한 의존성 매니저. 쉽게 말해 오픈소스 라이브러리를 프로젝트와 연결시키면서 개발자들이 편리하게 사용할 수 있게 해주는 환경 또는 도구.

**좋은 점**   
<p>
- 업데이트 명령어를 실행하면 프로젝트 버전에 맞춰서 설정한 오픈소스 라이브러리를 업데이트해 버전 관리하기가 편하다<br>
- 되도록이면 CocoaPods에 등록되어 있는 라이브러리들을 설치 권장
</p>


# CocoaPods 설치
```
$ sudo gem install cocoapods
```

## CocoaPods 설정하기
1. 프로젝트 생성
2. 워크스페이스(workspace) 생성
3. 워크스페이스에서 오른쪽 버튼을 눌러 Add Files to “”… 메뉴를 클릭해서 작업할 프로젝트를 워크스페이스 안에 추가함
4. root 프로젝트 폴더에서 podfile를 생성하여 연결할 오픈소스 라이브러리를 추가함
5. terminal에서 pod install를 입력하여 연결시킴
```
$ pod install
```

or

1. 프로젝트를 생성한 다음, 프로젝트 폴더에서 pod init 명령어를 실행 -> 알아서 Podfile이 생성되고 워크스페이스가 생김
```
$ pod init
```
2. Podfile을 설정한 다음 pod install 명령어를 쳐서 실제 프로젝트와 연결함
```
$ pod install
```

### Podfile 예시

```
platform :ios, '13.0'
use_frameworks!

target 'MyApp' do
  pod 'Alamofire', '~> 5.0'
  pod 'SwiftyJSON', '~> 4.0'
end
```
<br>
설치하면서 로그 보고 싶다면 아래 명령어처럼 뒤에 <code>--verbose</code> 붙이면 된다.   
```
$ pod install --verbose   // Show more debugging information
```
<br>
# CocoaPods 관련 오류
## 1. 특정 파일이 안 보인다.

<code>"/Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/SDKs/iPhoneOS.sdk/usr/include/sqlite3.h" not found"</code>

-> cocoapods 재설치
```
$ sudo gem install cocoapods
$ pod install --verbose
```
<br>
**참고**   
-> Xcode.app 과 Xcode-beta.app은 다르므로, Xcode 프로그램 이름과 프로그램이 위치해 있는 디렉토리 잘 확인하자! (Xcode 프로그램 응용 프로그램 폴더에 넣기)


## 2. 특정 module 빌드 에러

<code>"Could not build module firebase core" Error</code>

-> Xcode 종료   
-> project's temp file 삭제 (~/Library/Developer/Xcode/DerivedData --- Xcode->Preference->Location에 위치해 있음)   
-> ProjectName.xcworkspace 삭제   
-> Podfile.lock 파일이랑 Pods 폴더 삭제   
-> pod install 실행 (터미널)   
-> 새로 생성한 ProjectName.xcworkspace 실행하여 다시 빌드하기   

-> 그래도 안된다면?   
---> pod update   
(or) ---> pod --version 체크   
(or) ---> pod repo update   
        ---> Podfile에 'Firebase' 주석 처리   
        ---> pod install (old Firebase가 제거된다)   
        ---> Podfile에 'Firebase' 주석 해제   
        ---> pod install (new Firebase 설치)    --------> 해결 완료!   

### Reference
[stackoverflow](https://stackoverflow.com/questions/41709912/error-could-not-build-objective-c-module-firebase){:target="_blank"}   
[groups-firebase](https://groups.google.com/forum/#!topic/firebase-talk/Fu51jfAxh-E){:target="_blank"}

## 3. CocoaPods 설치 후 콘솔 로그 에러

<code>"The <project name [Debug/Release]> target overrides the 'ALWAYS_EMBED_SWIFT_STANDARD_LIBRARIES' build setting defined in 'Pods/Target Support Files/Pods-<project name>/Pods-<project name>.debug(or release).xconfig'. This can lead to problems with the CocoaPods installation."</code>

-> Targets -> Build Settings -> Build Options -> Always Embed Swift Standard Libraries -> Other... -> $(inherited)   
-> pod install

### Reference
[CocoaPods Issue](https://github.com/CocoaPods/CocoaPods/issues/5981){:target="_blank"}   
[stackoverflow](https://stackoverflow.com/questions/41570233/whats-always-embed-swift-standard-libraries-with-cocoapods-swift-3-and-xcode){:target="_blank"}

## 4. 특정 라이브러리만 업데이트하기
```
pod update {Pod Name}
```