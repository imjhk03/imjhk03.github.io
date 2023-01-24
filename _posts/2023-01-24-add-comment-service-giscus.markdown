---
title: 블로그에 댓글 기능 추가하기, Giscus
layout: post
category: [Blog]
img_path: /assets/img/2023/01/24/
tags: [jekyll]
---

오랜만에 사용하는 jekyll 테마 업데이트하면서 튜토리얼 글을 보니깐 댓글을 쓸 수 있는 것을 보았다. 보니깐 Giscus comments를 지원한 것이었다. 원래는 댓글 기능에 대해서 생각하지 않았는데, 신기하고 jekyll 테마가 지원하는 것을 하나씩 추가해 보는 게 재밌을 것 같아서 적용해 보았다. 처음으로 사용한 기능이어서 어떻게 적용했는지 간략하게 작성했다.

>사용하는 jekyll 테마마다 댓글 기능을 지원하거나 추가하는 내용이 다를 수 있기 때문에, 사용하는 jekyll 테마 공식 사이트 가서 확인해 보는 것을 권장한다.
{: .prompt-info }

# 댓글 기능
내가 사용하는 jekyll 테마인 [Chirpy](https://github.com/cotes2020/jekyll-theme-chirpy)는 5.1.0 버전부터 giscus 추가해서 총 3개의 댓글 서비스를 지원하고 있다. [disqus](https://disqus.com/), [utterances](https://utteranc.es/), [giscus](https://github.com/giscus/giscus) 이렇게 3개이다.

![Chirpy 튜토리얼 블로그 글에 있는 댓글 기능, giscus](image1.png)

3개 중에 giscus를 선택한 이유는 가장 최근에 지원한 댓글 기능이고, 조금 더 찾아보니깐 utterances는 github issues를 사용해서 댓글을 다는 거고 giscus는 github discussion을 사용해서 댓글을 단다. 이슈보다는 discussion이 조금 더 리파지토리 관리가 편할 것 같아서 적용했다. 물론 둘 다 깃헙 계정이 있어야 댓글을 달 수 있는데, 내 블로그는 주로 개발자들이 읽을 것 같고, 보통 깃헙 계정이 있지 않을까 해서 큰 어려움이 없을 것 같았다.

# Giscus 설치하기
적용하는 방법은 간단했다. 하지만 사전 조건이 있다.
1. [공개](https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/managing-repository-settings/setting-repository-visibility#making-a-repository-public) 저장소여야 합니다. (public repository)
2. [giscus](https://github.com/apps/giscus) 앱이 설치되어 있어야 합니다.
3. Discussions 기능이 [해당 저장소에서 활성화되어 있어야 합니다.](https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/enabling-features-for-your-repository/enabling-or-disabling-github-discussions-for-a-repository)


[Giscus 사이트](https://giscus.app)로 가서 giscus 설정에 필요한 정보들을 입력하면 된다. 그러면 giscus 설정에 필요한 값들을 보여주는데, 이 값들을 jekyll 테마에 맞게 반영하면 된다.

![Giscus 사이트에 있는 설정 화면. 언어, 저장소 주소, 페이지와 Discussions 연결에 대한 설정이 나와 있다.](image2.png)

위에서 사전 조건이 맞는다면 저장소 입력하는 공간에 저장소 주소를 입력한다. 페이지와 Discussions 연결을 선택하면 되는데, 기본값으로 경로가 선택되어 있다.

Discussion 카테고리도 선택해야 댓글이 정상적으로 discussion에 작성이 되는데, 카테고리 유형은 Announcements 유형으로 하는 것을 권장한다고 되어 있다. 나는 여기서 Comments라는 카테고리 새로 만들어서 적용했다.

![Dicsussion 카테고리 설정 화면](image3.png)

기타 기능이나 테마도 선택할 수 있고, 설정하면 아래와 같이 웹 사이트 템플릿에 적용할 수 있는 script 태그가 나타난다.

![설정한 내용들이 나와 있는 script 태그 코드가 나타난다.](image4.png)

적용하는 템플릿에 알맞게 적용하면 되고, 나는 여기서 Chirpy 테마에서 필요한 값들만 적용해서 반영했다.

![블로그에 댓글 기능 추가한 화면](image5.png)

# 마치며
사용하는 jekyll 테마가 댓글 기능을 지원해서 적용해 보았는데, giscus라는 서비스도 알게 되었다. 앞으로 jekyll 테마에서 지원하는 기능들을 하나씩 적용하면서 블로그를 조금 더 가꾸려고 한다.