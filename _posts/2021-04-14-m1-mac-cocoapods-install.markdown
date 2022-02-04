---
title: M1 mac에서 cocoapods 설치하기
layout: post
author: Joohee Kim
tags: cocoaPods
---

**Update:** Homebrew를 이용해서 설치하는 방법 추가하고 글 내용을 조금 수정했습니다.

<br>
M1 맥 미니 구매하고 나서, 놀라운 퍼포먼스와 무소음 환경을 즐기고 있습니다. Xcode에서 빌드하기 위해, 아직 SPM으로 옮기지 못 한 라이브러리 이용하기 위해서 cocoapods를 설치해야 했습니다. 하지만 인텔 기반 맥에서 cocoapods 설치하는 방법과 달랐는데, 오늘 이 포스트에서 소개하려고 합니다.

## Rosetta를 이용하고 ffi 설치해서 cocoapods 설치하는 방법
### 1. 터미널 앱을 Rosetta를 사용하여 열기
응용 프로그램에서 터미널 앱에서 오른쪽 클릭을 해 정보 보기를 한 후, 'Rosetta를 사용하여 열기'를 체크합니다.
![터미널 정보 창에 Rosetta를 사용하여 열기를 체크](/assets/img/2021/04/14/image1.PNG)

### 2. 터미널 앱을 열어서 아래 명령어 입력하기
```zsh
sudo gem install ffi
```

### 3. Cocoapods 설치 혹은 pod install
`pod install` 명령어를 입력해서 설치하거나, cocoapods이 설치되어 있지 않다면 `sudo gem install cocoapods` 명령어를 입력하면 됩니다.

## Homebrew를 이용해서 설치하는 방법
최근에 homebrew를 이용해서 개발할 때 필요한 패키지들을 설치하는데, homebrew를 이용해서 cocoapods를 설치할 수 있습니다.
아래 명령어를 입력하면 설치가 됩니다. ([Homebrew cocoapods 사이트](https://formulae.brew.sh/formula/cocoapods))
```zsh
brew install cocoapods
```

<br>
애플에서 Swift Packaage Manager를 지원하고 있기 때문에, SPM으로 설치하는 것을 권장합니다. 하지만, 아직 SPM을 지원하지 않는 라이브러리를 사용하고 있다면 cocoapods를 설치해서 사용해야 합니다.
위 2가지 방법 중 하나로 cocoapods를 설치하면 사용할 수 있습니다.

관련 이슈는 아래 깃헙 주소 참고하여 보시면 되겠습니다.

**참고**
<br>
[CocoaPods compatibility with Apple DTK (Apple Silicon) · Issue #9907 · CocoaPods/CocoaPods · GitHub](https://github.com/CocoaPods/CocoaPods/issues/9907)