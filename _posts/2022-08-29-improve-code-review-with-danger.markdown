---
title: 코드 리뷰를 개선할 수 있는 GitHub Actions를 이용해서 Danger + SwiftLint 도입기
layout: post
tags: [userdefaults, foundation]
---

깃허브에서 PR을 올려서 코드 리뷰를 받고 있는데, 코드 리뷰를 하다 보면 코드 스타일 등에 대해서 놓친 경우가 있어서 코멘트를 다는 경우가 있다. 예를 들면 네이밍 컨벤션이 잘 지켜지지 않거나 자주 놓치는 접근 제한자 같은 것도 있다.

코드 리뷰를 하는데 이런 코드 스타일에 대해서 코멘트를 다는 시간을 줄이고 코드 리뷰를 더 효율적으로 도와줄 수 있는 Danger가 있다.

## Danger
[Danger](https://danger.systems/swift/)는 CI 프로세스에 실행하는 도구로, 코드 리뷰 관련 작업을 자동화 작업을 진행한다. `Dangerfile`이라는 파일을 가지고 어떤 동작을 수행할지 작성한 다음에 사용한다. GitHub, GitLab, Jenkins, Bitrise 등 지원하는 서비스가 많다. 나는 여기 중에서 GitHub Actions를 이용해서 PR을 올릴 때, Danger 작업을 실행하면서 코드 스타일에 대해서 검사하도록 추가하는 작업을 소개하겠다.

<img src="/assets/img/2022/08/29/img1.jpeg" alt="Danger GitHub 사이트를 가면 'Stop saying you forgot to ...' in code review 설명이 있다."/>

# GitHub Actions
Danger를 실행하는 방법 중에 하나는 [GitHub Actions](https://docs.github.com/en/actions)를 사용하는 것이다. GitHub Actions는 깃허브에서 제공하는 CI와 CD를 위한 서비스입니다. 깃허브에 있는 프로젝트 저장소에서 어떤 이벤트를 발생하면 어떤 동작을 실행하도록 한다. GitHub Action을 사용하려면 아래와 같이 워크플로우를 생성해야 한다. [깃허브 문서](https://docs.github.com/en/actions/quickstart)에도 자세히 설명되어 있어 참고하면 좋다.

> 작업하기 전에 신규 브랜치 생성해서 아래 작업을 진행하면 좋다.

1. 깃허브 리파지토리에 `.github/workflows` 디렉토리를 생성한다.
2. `.github/workflows` 디렉토리 안에 `danger-swiftlint.yml` 파일을 만든다. 파일 이름은 꼭 이 이름 사용 안 해도 괜찮다.
3. 아래에 나와 있는 YAML 내용을 복사해서 아까 생성한 danger-swiftlint.yml 파일에 작성한다.

```yaml
// 1
name: Danger-SwiftLint
on:
  // 2
  pull_request:
    branches: [ main, develop ]

jobs:
  build:
    runs-on: ubuntu-latest
    name: "Run Danger"
    steps:
      - uses: actions/checkout@v1
      - name: Danger
        // 3
        uses: docker://ghcr.io/danger/danger-swift-with-swiftlint:3.13.0
        with:
            args: --failOnErrors --no-publish-check
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```
1.

## Dangerfile
지금까지는 PR을 올렸을 때 깃허브 액션을 실행하는 워크플로우 파일을 생성했다. 이제는 워크플로우에서 Danger를 실행하라고 했으니, Danger를 통해서 어떤 작업을 수행할지 Dangerfile을 configure하는 작업이 필요하다.

터미널을 이용해서 Dangerfile 파일을 생성하고 edit하는 명령어를 사용하면 된다. 아래와 같이 Danger를 설치하고 나서 터미널 명령어를 치면 Xcode에서 Dangerfile을 열게 된다.

// 터미널 명령어 추가
// Xcode에서 열린 Dangerfile 화면 이미지 추가

아래와 같이 Dangerfile을 구성하면 된다.

// 관련 설명 추가

그리고 나서 안전하게 터미널에서 엔터를 치고 나온다. 현재까지 작업을 진행했다면 아래 두 가지 파일을 생성했을 것이다.

1. github actions 파일
2. Dangerfile 파일

관련 내용이 반영이 되었다면 작업한 내용을 push하고 PR을 올려서 리파지토리에 반영하도록 한다. 올바르게 작업을 했다면 PR을 생성한 순간 Danger가 실행할 것이다.

// Danger 실행 화면 이미지 추가
// Danger SwiftLint 관련 코멘트 화면 이미지 추가

## SwiftLint

## CodeCoverage


## Conclusion
