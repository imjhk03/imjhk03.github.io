---
title: git switch 명령어
layout: post
category: [Git]
tags: [git]
---

Git에서 브랜치 변경할 때는 주로 `git checkout`을 사용한다. 최근에 `git switch` 명령어를 알게 되었는데, `git checkout` 명령어랑 뭐가 다른지 정리해 봤다.

## git switch 명령어가 나오게 된 이유
Git 2.23에서 `checkout` 대신할 `switch`, `restore` 명령어가 나왔는데, `checkout` 명령어가 너무 많은 기능을 가지고 있어서 새로운 명령어들이 나왔다.

* `checkout`: 브랜치를 이동하거나 working tree 파일을 복원
* `switch`: 브랜치 이동
* `restore`: Working tree 파일 복원

`git checkout` 명령어가 하는 일을 `git switch`, `git restore`로 분리한 것으로 보인다.

## 사용법
다른 브랜치로 변경하고 싶을 때
```
git switch other-branch
```

새로운 브랜치 만들어서 변경하고 싶을 때
```
git switch -c new-branch
```

특정 커밋에서 새로운 브랜치 만들어서 변경하고 싶을 때
```
git switch -c new-branch <start-point(커밋 번호)>
```

만약 변경하려는 브랜치와 충돌하는 로컬 수정 사항이 있는 경우, 로컬 변경 사항의 작업을 지우도록 할 수 있다. (사용할 때 주의 필요!)
```
git switch other-branch --discard-changes
```

이전에 checkout 한 브랜치로 다시 변경하고 싶을 때
```
git switch -
```

<br>
지금 작업 환경에서 특별히 `switch` 명령어를 사용할 일은 아직까지는 없을 것 같다. 그래도 새로운 명령어가 나와서 한번 정리해 봤다.

<br>
**참고**
<br>

[Git - git-switch Documentation](https://git-scm.com/docs/git-switch)

[새 버전에 맞게 git checkout 대신 switch/restore 사용하기 :: Outsider's Dev Story](https://blog.outsider.ne.kr/1505)

[git switch - Switching branches](https://www.git-tower.com/learn/git/commands/git-switch)