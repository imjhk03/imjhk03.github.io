---
title: Fork한 Repository Sync하기 (동기화하기)
layout: post
tags: git
---

예전에는 리파지토리를 fork 해서 사용해 본 적이 드물었는데, 최근에는 fork 해서 개인 리파지토리에서 개발하다가 upstream 리파지토리로 반영하는 일이 잦아들었다. 그래서 개발하다 보면 최신 상태로 동기화 작업을 해야 하는데 맨날 까먹어서 글로 남기려고 한다.

1. 터미널 연다.
2. 작업하고 있는 리파지토리 경로로 이동한다.
3. upstream 리파지토리의 브랜치들을 fetch한다.
```
$ git fetch upstream
```
4. fork한 리파지토리의 로컬 기본 브랜치로 이동한다. 예를 들면 ```master``` 혹은 ```main```.
```
$ git checkout main
```
5. upstream 기본 브랜치를 병합한다.
```
$ git merge upstream/main
```
