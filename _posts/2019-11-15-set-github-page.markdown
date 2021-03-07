---
title: 깃허브(GitHub) 블로그 구축하기
layout: post
author: Joohee Kim
description: GitHub Page 구축하면서 삽질한 이야기
tags: blog
---

개발자들이라면 한 번이라도 봤을 만한 블로그 주소는 {username}.github.io 일 것이다. 개인적으로 깃허브 블로그 페이지 만들면서 고생해서 간단하고 최소한의 작업으로 구축하는 방법을 기록하기 위해 포스트를 쓰기로 했다.

먼저 GitHub 계정으로 새로운 리파지토리(repository)를 만들면 된다. 자세한 설명은 [GitHub Pages][githubpage]{:target="_blank"} 사이트 가면 된다.

![GitHub Page example](/assets/img/2019/11/16/image1.png)
영어로 되어 있지만, 간단하기 때문에 문제없을 것 같다.

Hello World 까지 찍히는 거 보면 이제부터는 테마를 이용해서 꾸미는 작업이 필요하다. 보통 지킬(jekyll) 이용해서 테마를 입히는데, 테마를 입히기 위해서는 필요한 명령어들이 있다. 먼저 그 명령어들을 사용할 수 있게 기본 세팅을 설정했다. 사용하는 터미널(Terminal) 프로그램 열고 아래와 같은 명령어들을 실행하면 된다. [Jekyll][jekyllPage]{:target="_blank"} 페이지 들어가보면 더 자세한 설치 방법과 적용 방법들이 있다.

```terminal
$ gem install bundler jekyll
$ jekyll new my-awesome-site
$ cd my-awesome-site
~/my-awesome-site $ bundle exec jekyll serve
```

만약 bundler가 없으면 아래 명령어로 설치하면 된다.

```terminal
$ gem install bundler
```

웹 페이지에 필요한 jekyll 파일들 설치하고 브라우저로 http://localhost:4000 에 접속하면 확인할 수 있다.

[Jekyll Themes][jekyllThemes]{:target="_blank"} 페이지 혹은 Github에 jekyll 테마 검색해서 마음에 드는 테마를 고르면 된다. 여기서 [Texture][textureTheme]{:target="_blank"} 테마를 입히기로 했다.

![Texture Jekyll Theme Demo](/assets/img/2019/11/16/image2.png)
깔끔 깔끔

테마마다 적용하는 방법이 다르다. Github 페이지의 README 참고하여 적용하면 된다. 테마마다 사용할 수 있는 설정들이 있어 적용하면서 마음에 들지 않으면 다른 테마 찾아서 적용해도 좋을 것 같다.

천천히 조금씩 설정하여 아래와 같이 초기 화면을 만들었다.

![Github Page Jekyll Theme First Look](/assets/img/2019/11/16/image3.png)

Jekyll 테마 입히고 http://localhost:4000 에 접속하여 미리 어떤 모습으로 보일지 확인할 수 있다. 먼저 bundle 설치하고 bundle exec jekyll serve 명령어 실행하면 된다.

```terminal
$ bundle install
$ bundle exec jekyll serve
```

페이지 추가하고 편집하면서 저장하면, 실행 중에 변경된 내용을 확인할 수 있다. 하지만 _config.yml 파일을 변경했으면 테스트로 실행 중인 것을 멈추고 다시 서버를 가동하여 확인해야 한다.

![Terminal Process of bundle exec jekyll serve](/assets/img/2019/11/16/image4.png)


정말 최소한의 구축 설정으로 한 것이다. 갓허브로 리파지토리 만들고, jekyll 설정하여 기본 세팅을 한 뒤, 마음에 드는 jekyll 테마를 찾아서 입히는 작업까지만 했다. 앞으로 포스팅하면서 더 이쁘게 꾸미는 방법을 찾으면서 깃허브 블로그 관리하도록 노력해야겠다. 이 짧은 글이 처음으로 깃허브 블로그 구축하는 분들께 도움이 되길 바라며, 다음 포스트에서 볼 수 있으면 좋겠다.


[githubpage]: https://pages.github.com
[jekyllPage]: https://jekyllrb.com
[jekyllThemes]: http://jekyllthemes.org
[textureTheme]: https://github.com/thelehhman/texture