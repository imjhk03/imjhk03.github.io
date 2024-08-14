---
title: 이미 git으로 관리하고 있는 파일을 .gitignore에 추가했을 때, 변경해도 더 이상 추적하지 않도록 하는 방법
layout: post
categories: [Git]
tags: [git]
---

Git으로 관리하는 파일을 어느 순간 .gitignore에 추가하는 경우가 있다. 이때 이 파일을 수정해도 git status에 나타나는 경우가 있는데, 이럴 때 추적을 더 이상 하지 않도록 아래 명령어를 사용하면 된다.

```
git rm -r --cached .
git add .
git commit -am "Remove ignored files"
```

위 명령어를 사용하면 이미 tracking 하고 있는 파일들을 원격 저장소에서 삭제가 된다. 원격 저장소에는 그대로 놔두려면 다른 방법을 사용해야 한다. 나는 주로 원격 저장소까지 추적하지 않도록 삭제해서 반영하고 싶어서 이 방법을 자주 사용한다.

<br>
참고

[How do I make Git forget about a file that was tracked, but is now in .gitignore?](https://stackoverflow.com/questions/1274057/how-do-i-make-git-forget-about-a-file-that-was-tracked-but-is-now-in-gitignore)