---
title: Xcode Playground에서 attach failed invalid argument 에러 해결하는 방법
layout: post
tags: [swift playground, developer tools]
---

Xcode Playground 생성해서 간단한 코드를 실행하려고 하는데, 아래와 같이 에러가 발생해서 실행이 안 되는 것을 발견했다.

> Failed to launch process. Failed to attach to stub for playground execution: error: attach failed ((os/kern) invalid argument).

![Xcode Playground Error showing "Failed to launch process"](/assets/img/2022/04/04/image1.png)

에러가 발생한 이유는 Xcode를 'Rosetta를 사용하여 열기' 설정을 활성화해서 발생했다. 해당 설정을 비활성화하니깐 에러가 발생하지 않았다.

<br>
**참고**

[Xcode Playground Failed to launch](https://stackoverflow.com/questions/69829731/xcode-playground-failed-to-launch)