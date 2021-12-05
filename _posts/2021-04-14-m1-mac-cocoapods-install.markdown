---
title: M1 mac에서 cocoapods 설치하기
layout: post
author: Joohee Kim
tags: cocoaPods
---

M1 맥 미니 구매하고 나서, 놀라운 퍼포먼스와 무소음 환경을 즐기고 있다. Xcode에서 빌드하기 위해, 아직 SPM으로 옮기지 못 한 라이브러리 이용하기 위해서 cocoapods를 설치해야 했다. 하지만 인텔 기반 맥에서 cocoapods 설치하는 방법과 달랐는데, 오늘 이 포스트에서 소개하려고 한다.

## 1. 터미널 앱을 Rosetta를 사용하여 열기
응용 프로그램에서 터미널 앱에서 오른쪽 클릭을 해 정보 보기를 한 후, 'Rosetta를 사용하여 열기'를 체크합니다.
![터미널 정보 창에 Rosetta를 사용하여 열기를 체크](/assets/img/2021/04/14/image1.PNG)

## 2. 터미널 앱을 열어서 아래 명령어 입력하기
```
sudo gem install ffi
```

## 3. Cocoapods 설치 혹은 pod install
`pod install` 명령어를 입력해서 설치하거나, cocoapods이 설치되어 있지 않다면 `sudo gem install cocoapods` 명령어를 입력하면 된다. 그러면 끝~

<br>
언젠가는 native하게 cocoapods 설치하는 날이 오길 바라며, 추가적인 업데이트 상황이 궁금하시다면 아래 깃허브 이슈 페이지 참고하면 된다.

참고: [CocoaPods compatibility with Apple DTK (Apple Silicon) · Issue #9907 · CocoaPods/CocoaPods · GitHub](https://github.com/CocoaPods/CocoaPods/issues/9907)