---
title: 깃허브(GitHub) 블로그 jekyll 테마 커스텀(custom)하기
layout: post
date: '2019-11-17 22:30:00'
tags: blog
description: Jekyll Theme 프로젝트를 fork해서 커스텀하기
---

어제 깃허브 블로그 구축하고 나서 꾸미려고 하니깐 뭔가 마음대로 꾸밀 수 없는 걸 느껴서, 테마를 가져와서 내 입맛대로 꾸밀 수 있을까 찾다가 jekyll theme을 fork 해서 커스텀할 수 있는 방법이 있다고 했다. 오늘 포스트는 테마를 조금 커스텀 할 수 있는 부분에 대한 내용을 담았다.

소스에 있는 세팅들 보다가 수정하고 싶은 부분이 생겼는데, css 를 건드릴 수 있는 부분이 없었다. Padding을 줄이거나, icon을 추가하거나 등 세팅할 수 있는 부분을 찾을 수가 없었다.

![GitHub Page Folder withou css folder](/assets/img/2019/11/17/image1.png)

(css 관련된 부분들을 찾을 수가 없다)

깃헙 블로그 테마를 적용하는 방법이 두 가지가 있었는데, 어제 구축한 방법은 테마를 기본적으로 그냥 가져와서 세팅하는 방법이다. 굳이 커스텀 할 필요를 못 느낄 정도로 테마가 잘 되어 있으면 이 방법을 쓰면 된다. 하지만 뭔가 조금 더 꾸미고 싶은 부분이 있다면, 해당 테마를 `fork` 해서 가져와서 조금 수정해서 쓰는 방법이 있다. Fork는 리포지토리를 다 복사해서 새로운 리포지토리로 붙여놓는다고 보면 된다 (Copy and Paste).

현재 적용하고 있는 Texture GitHub 프로젝트 가서 상단에 있는 Fork 누르면 본인 계정에 새로운 리포지토리가 생성하면서 fork가 된다.

![GitHub Project menu that has fork](/assets/img/2019/11/17/image2.png)

Fork가 다 되면 새로운 리포지토리가 생성이 되어 있고, 해당 리포지토리의 Setting으로 가서 리포지토리 이름을 다시 {username}.github.io 로 변경한다.

![Forked project and changed name](/assets/img/2019/11/17/image3.png)

Fork 한 프로젝트 안의 파일들을 보면 테마마다 다르겠지만 assets, layouts, includes 등 처음에 그냥 테마 설치하여 적용했을 때보다 많은 폴더가 있다. 여기서부터는 각자 적용한 테마들의 커스텀 하는 방법 참고하여 진행하면 된다. README 파일에 설명되어 있을 수 있고, 아니면 직접 소스 분석하면서 수정하면 된다. 아래부터는 **Texture jekyll theme** 한해서 이 깃헙 블로그에 적용한 것들을 일부 소개하고 있다.

--------------------------------------------------------------------------------------------------------------------------------------------------------------------------

### Texture jekyll theme custom - 1. instagram icon 추가
적용한 테마 디자인 중 twitter, github 등 소셜 링크 추가한 것들이 있는데, 개인적으로 인스타그램도 넣으면 어떨까 하고 소스 분석해봤다. page_header.html 파일에 관련한 소스들이 있어 instagram 계정 추가하고 개발 서버로 한번 돌렸는데 인스타그램 아이콘이 나타나지 않았다.

```html{% raw %}
{%- if site.texture.social_links.instagram -%}
	<a href="https://instagram.com/{{ site.texture.social_links.instagram }}"><li><i class="icon-instagram"></i></li></a>
{%- endif -%}
{% endraw %}
```

관련해서 더 찾다 보니 scss 파일을 통해서 아이콘을 적용하는 것 같은데, 아이콘 파일들은 assets에 없었다. 유심히 소스 보니깐 fontello 폰트를 사용하는 거로 봐서, fontello 폰트 안에 있는 아이콘을 쓰는 게 아닐까 생각이 들었다. Fontello 사이트 들어가서 확인해보니깐 아이콘들이 있었으며, 기존에 사용하고 있는 트위터와 깃허브 아이콘들이 있었다.

![Fontello website](/assets/img/2019/11/17/image4.png)

(Fontello website)

원하는 instagram 아이콘 선택해서 다운로드하고 font 폴더에 있는 것들을 덮어쓰기 하니깐 이제는 기존에 나오는 아이콘들이 나타나지 않았다. 다운받은 폰트 관련 파일 안에는 이전에 적용되어 있던 아이콘에 대한 설정들이 없었고 인스타그램 아이콘만 관련된 설정들이 있어서 나타나지 않았던 것이었다. 다시 관련된 아이콘들을 찾아서 포함하여 다운받고 다시 적용하니깐 잘 나왔다.

![Only instagram icon shown](/assets/img/2019/11/17/image5.png)

(instagram 아이콘은 나타났지만, 이전에 나타났던 아이콘들은 나오지 않았다)

![Fontello font settings](/assets/img/2019/11/17/image6.png)

(새로 다운받은 폰트 관련 파일 중 하나. 관련된 아이콘들에 대한 설정들이 있다)

![All icon shown](/assets/img/2019/11/17/image7.png)

(새로 다운받은 폰트 관련 파일들을 적용하고 나서 이전에 나타난 아이콘과 새로운 인스타그램 아이콘이 나온다)

Jekyll 테마마다 다 다르겠지만, 지금의 테마에서는 fontello 폰트 이용하여 아이콘들을 사용하고 있기 때문에 새로운 아이콘 적용하고 싶으면 fontello 사이트에 들어가서 **기존 + 새로운 아이콘** 포함하여 새로 다운 받아야 한다. 다운받은 뒤, 관련 파일들을 다 적용하고 css 관련해서 추가하면 된다.


### Texture jekyll theme custom - 2. 모바일 환경에서 날짜가 제목 밑으로 나타나기
깃허브 블로그 만들고 나서 기쁜 마음으로 침대에 누우면서 잠들기 전에 스마트폰으로 들어갔는데, 날짜 관련 부분이 제목 옆으로 나와서 뭔가 이뻐 보이지 않는 느낌이 들었다.

![Mobile github page screenshot](/assets/img/2019/11/17/image8.PNG)

(밑으로 내려가 줘....)

이 부분 때문에 jekyll 테마를 커스텀 하고 싶었던 첫 번째 이유이기도 했다. 오랜만에 css 관련 속성들을 찾으면서 적용하고 수정하고 다시 적용하면서 관련 디자인 부분을 추가했다. 모바일 환경에서만 적용이 필요하기 때문에 관련된 부분도 찾으면서 적용했다. 아래는 css 관련 추가한 코드다.

```css
@media screen and  (max-width: $mobileWidth) {
    display: block;
}
```

![Mobile github page screenshot 2](/assets/img/2019/11/17/image9.png)

(왼쪽은 적용 전, 오른쪽은 적용 후. 태그 관련된 부분은 아래에 다룰 예정이다)



### Texture jekyll theme custom - 3. 포스트 관련 태그 나타나기
포스트 안에 태그들이 있는데, 이 태그들이 포스트 목록에도 나오면 좋을 것 같은 생각이 들어서 관련 내용 추가하기로 했다.

일단 태그 관련된 부분을 나타내기 위해 div 태그 새로 추가해서 tag 가 나오게 했다.

```html{% raw %}
<div class="post-tag">
 <ul class="post-tag-container">
  {%- for tag in post.tags -%}
    <li>tag</li>
	{%- endfor -%}
 </ul>
</div>
{% endraw %}
```


그리고 나서 태그와 포스트 짧은 본문 내용 부분과 떨어지도록 아래 css style 추가했다.

```
.post-tag {
	margin-top: 5px;
	display: inline-block;
	margin-bottom: -10px;
}
```

마지막으로 포스트 목록에 나타나는 태그들 관련 css style 추가하여 완성했다.

![Post tags screenshot 3](/assets/img/2019/11/17/image10.png)

(왼쪽은 적용 전, 오른쪽은 적용 후. 이제 태그들만 보고 어떤 내용인지 예상할 수 있고, 보고 싶은 내용만 볼 수 있어서 좋을 듯하다)


### 마무리
마음에 드는 테마들이 있지만, 조금 더 커스텀 할 수 있도록 fork 하는 방법과 예시로 texture 테마에서 수정한 몇 가지들을 소개해 봤다. 더 적용해야 하는 부분들이 있지만, 일단 지금까지만 수정하고 차근차근하도록 해야겠다. 그리고 오랜만에 css 공부하니깐 굉장히 어색했다.