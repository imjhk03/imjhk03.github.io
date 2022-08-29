---
title: 코드 리뷰를 개선할 수 있는 Danger + SwiftLint Plugin
layout: post
tags: [optimization]
---

깃허브에서 PR을 올려서 코드 리뷰를 받고 있는데, 코드 리뷰를 하다 보면 코드 스타일 등에 대해서 놓친 경우가 있어서 코멘트를 다는 경우가 있다. 예를 들면 네이밍 컨벤션이 잘 지켜지지 않거나 자주 놓치는 접근 제한자 같은 것도 있다.

코드 리뷰를 하는데 이런 코드 스타일에 대해서 코멘트를 다는 시간을 줄이고 코드 리뷰를 더 효율적으로 도와줄 수 있는 Danger가 있다. 이 글에서는 Danger가 무엇이고 GitHub Actions를 이용해서 PR을 올릴 때마다 코드 스타일이나 컨벤션을 체크할 수 있는 작업을 적용하는 방법을 소개하겠다.

## Danger
[Danger](https://danger.systems/swift/)는 CI 프로세스에 실행하는 도구로, 코드 리뷰 관련 작업을 자동화 작업을 진행한다. `Dangerfile`이라는 파일을 가지고 어떤 동작을 수행할지 작성한 다음에 사용한다. GitHub, GitLab, Jenkins, Bitrise 등 지원하는 서비스가 많다. 나는 여기 중에서 GitHub Actions를 이용해서 PR을 올릴 때, Danger 작업을 실행하면서 코드 스타일에 대해서 검사하도록 추가하는 작업을 소개하겠다.

<img src="/assets/img/2022/08/30/img1.jpeg" alt="Danger GitHub 사이트를 가면 'Stop saying you forgot to ...' in code review 설명이 있다."/>

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

// 3
jobs:
  build:
    runs-on: ubuntu-latest
    name: "Run Danger"
    steps:
      - uses: actions/checkout@v1
      - name: Danger
        // 4
        uses: docker://ghcr.io/danger/danger-swift-with-swiftlint:3.13.0
        with:
            args: --failOnErrors --no-publish-check
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```
파일에 대한 주요 설명은 아래와 같다. 각 코드에 대한 자세한 설명은 [깃허브 문서](https://docs.github.com/en/actions/learn-github-actions/understanding-github-actions#understanding-the-workflow-file)를 참고하면 된다.
1. 워크플로우 이름
2. 워크플로우를 실행(trigger)할건지 정의한다. 여기서는 pull_request를 만들 때 마다 워크플로우를 실행할 것이고, 브랜치는 main, develop일 경우에 워크플로우가 실행될 것이다.
3. 워크플로우에 실행할 작업들
4. Danger 작업을 진행할 때 어떤 것을 실행할지 정의한다. 여기서는 Danger + SwiftLint를 사용할 것이다.

지금까지는 PR을 올렸을 때 깃허브 액션을 실행하는 워크플로우 파일을 생성했다. 이제는 워크플로우에서 Danger를 실행하라고 했으니, Danger를 통해서 어떤 작업을 수행할지 `Dangerfile`을 configure하는 작업이 필요하다.

## Dangerfile
터미널을 이용해서 Dangerfile 파일을 생성하고 edit하는 명령어를 사용하면 된다. 아래와 같이 먼저 brew로 Danger를 설치한다.

```bash
brew install danger/tap/danter-swift
```

그리고 Dangerfile를 추가할 프로젝트 루트 경로에서 아래 터미널로 명령어를 친다. 아래 명령어를 치면 터미널에서 기다렸다가 Xcode로 Dangerfile을 생성하거나 편집하도록 열게 된다. 이때 터미널을 계속 열어둔다.

```bash
danger-swift edit
```

<img src="/assets/img/2022/08/30/image2.png" alt="터미널 창에 danger-swift edit 명령어를 치면 Dangerfile을 열면서 Dangerfile을 수정 완료하면 터미널을 닫히라는 안내 문구가 있다."/>

Xcode으로 열면 main.swift 파일을 열어서 Dangerfile 설정을 작성하면 된다.

<img src="/assets/img/2022/08/30/image3.png" alt="Xcode에서 연 Danger 폴더 경로 중에 main.swift 파일을 찾아서 열어둔다."/>

아래와 같이 Dangerfile을 구성하면 된다. 아래는 간단한 예제를 작성한 것이며, [다른 예제](https://danger.systems/swift/tutorials/ios_app.html)들을 참고하면서 프로젝트에 맞는 규칙들을 추가하면 된다.

```
import Danger

let danger = Danger()

// Make it more obvious that a PR is a work in progress and shouldn't be merged yet
if danger.github.pullRequest.title.contains("WIP") {
    warn("PR is classed as Work in Progress")
}

// Warn when there is a big PR
if (danger.github.pullRequest.additions ?? 0) > 500 {
    warn("Big PR, try to keep changes smaller if you can")
}

// Run Swiftlint
SwiftLint.lint(inline: true, configFile: ".swiftlint.yml")
```

그리고 나서 Xcode를 먼저 끈 후에, 안전하게 터미널에서 엔터를 치고 나온다. 현재까지 작업을 진행했다면 아래 두 가지 파일을 생성했을 것이다.

1. github actions 파일
2. Dangerfile 파일

관련 내용이 반영이 되었다면 작업한 내용을 push하고 PR을 올려서 리파지토리에 반영하도록 한다. 올바르게 작업을 했다면 PR을 생성한 순간 Danger가 실행할 것이다.

<img src="/assets/img/2022/08/30/image4.png" alt="깃허브에 PR을 올렸을 때 SwiftLint를 검사한 결과를 Danger에서 코멘트를 달아준다."/>

## SwiftLint 및 기타 plugins
[SwiftLint](https://github.com/realm/SwiftLint)는 스위프트 스타일과 컨벤션 등을 지키도록 도와주는 도구다. Danger SwiftLint plugin을 가지고 pr 올릴 때마다 체크해 줘서 막강한 힘을 보여준다. SwiftLint 말고도 다른 Danger plugin들도 있다. 예를 들어 빌드 정보를 보여주거나 테스트 코드들을 실행하는 등 다른 plugin들을 찾아보면서 프로젝트에 알맞은 도구를 사용하면 좋을 것 같다. [Plugin](https://github.com/danger/awesome-danger)들은 여기서 확인해 볼 수 있다.

## Conclusion
초반에 적용할 때는 시행착오를 많이 겪었지만, 적용하고 나서는 코드 리뷰할 때 코드 스타일이나 컨벤션 등을 덜 신경 쓰고 코드에 더 집중할 수 있는 것을 느꼈다. 앞으로도 좋은 도구들이 있으면 사용해서 더 효율적인 코드 리뷰를 진행할 수 있도록 연구해볼 계획이다.

<br>
**참고**

[Danger Swift](https://danger.systems/swift/)

[Danger Swift GitHub](https://github.com/danger/swift)

[Danger plugins to speed up code reviews](https://www.avanderlee.com/optimization/danger-plugins/)

[Automate PRs With Danger](https://agostini.tech/2019/06/09/automate-prs-with-danger/)