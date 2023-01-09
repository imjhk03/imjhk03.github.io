---
title: Multiple Pages Xcode Playground
layout: post
img_path: /assets/img/2023/01/09/
tags: [swift playground]
---
간단한 스위프트 코드를 작성할 때 Xcode Playground를 사용한다. Xcode Playground 파일을 생성하면 하나의 플레이그라운드에서 코드를 작성하게 되는데, 여러 플레이그라운드 파일들을 묶고 싶은 경우가 있다. 예를 들어, 하나의 주제를 가지고 다양한 코드 방법을 파일별로 정리하거나 문법 공부할 때 문법별로 정리하는 경우가 있다. 이럴 때 멀티 페이지를 만드는 방법을 소개하겠다.

## Playground Page 생성하기

Xcode에서 플레이그라운 만들고 나서 ```File > New > Playground Page```를 선택하면 하나의 플레이그라운드 프로젝트 안에 'Untitled Page' 와 'Untitled Page 2'가 생기는 것을 확인할 수 있다.

![Xcode File > New > Playground Page 메뉴를 보여주는 스크린샷](image1.png)

!['Untitled Page'와 'Untitled Page 2' 플레이그라운드 페이지가 생성된 화면](image2.png)

이런 식으로 하나의 플레이그라운드 안에서 여러 플레이그라운드 페이지를 만들 수 있다.

여기서 조금 더 문서처럼 사용할 수 있게 [마크업](https://commonmark.org) 문법을 활용해서 플레이그라운드 페이지를 정리하는 방법을 소개하겠다.

## 플레이그라운드에서 마크업 사용하기

### 목차 만들기

여러 플레이그라운드 페이지를 만들면 목차 역할을 하는 플레이그라운드 페이지를 만들면 편하다. 아래와 같이 Introduction 플레이그라운드 페이지를 만들고 마크업을 사용해서 텍스트 크기를 제일 크게 적용하도록 '```#```' 하나를 추가했다. 아직까지는 크게 변한 게 없다.

![Introduction이라는 Heading 1 텍스트를 가진 마크업이 작성되어 있다.](image3.png)

여기서 ```Editor > Show Rendered Markup```을 선택하면 아래와 같이 큰 타이틀이 가진 마크업이 나타난 것을 확인할 수 있다.

![Xcode Editor > Show Rendered Markup 메뉴를 보여주는 스크린샷](image4.png){: w="700" h="400" }

![마크업을 랜더링한 화면. 큰 텍스트 크기를 가진 Introduction이 나타난다.](image5.png){: w="700" h="400" }

자주 사용할 수 있기 때문에 ```Xcode Key Binding```을 이용해서 빠르게 마크업 랜더링 하는 것을 확인해 볼 수 있다. 아래와 같이 ```Xcode Settings > Key Binding```에서 markup 검색해서 설정하면 된다.

![Xcode Settings > Key Binding 화면에서 'markup'을 검색한 화면. 'Show Rendered Markup' Key Binding 항목이 나타난다.](image6.png)

목차를 순서가 있는 목록이나 순서가 없는 목록으로 만들어서 사용할 수 있다. 아래와 같이 숫자를 붙이거나 '```*```' 혹은 '```-```'를 사용하면 된다.

![Day 1, Day 2, Day 3를 목차별로 마크업 정리한 화면. Day 1 앞에는 숫자 1이 있고 Day 2는 *, Day 3는 -가 쓰여 있다.](image7.png){: w="700" h="400" }

![그 결과 Day 1 앞에만 숫자 1이 있는 목록이 나타나고, Day 2랑 Day 3는 숫자가 없는 목록으로 나타난다.](image8.png){: w="700" h="400" }

### 플레이그라운드 네비게이션 연결하기

마크업은 링크를 지원하기 때문에 웹사이트랑 이메일 링크를 추가해서 연결할 수 있다. 이런 링크를 이용해서 목차에 링크를 추가해서 플레이그라운드 페이지로 이동할 수 있게 가능해진다. 아래처럼 대괄호(```[ ]```)랑 소괄호(```( )```)를 이용해서 소괄호 안에 링크를 추가하면 된다. 링크는 띄어쓰기를 지원하지 않기 때문에 띄어쓰기가 플레이그라운드 제목에 있을 경우, '```%20```'으로 사용해야 한다.

![마크업으로 목차 안에 플레이그라운드 페이지를 링크로 걸려 있는 코드. Day 1과 Day 2의 제목을 대괄호 안에 있고, 소괄호 안에는 플레이그라운드 이름이 나타나 있다. 플레이그라운드 이름이 'Day 1', 'Day 2'로 되어 있어서 띄어쓰기는 '%20'으로 교체되어 있다.](image9.png){: w="700" h="400" }

![그 결과 링크가 걸려 있는 Day 1, Day 2 목록이 나타나 있다. 주황색으로 변경하면서 링크를 가진 텍스트라는 것을 알 수 있다.](image10.png){: w="700" h="400" }

새로 생긴 플레이그라운드 페이지를 보면 위에는 Previous랑 아래에는 Next가 있는 것을 볼 수 있다. 마크업 랜더링한 상태에서 누르면 페이지 이동이 되는 것을 확인할 수 있다.

![마크업으로 previous랑 next를 표시하는 화면. 링크를 사용하는 것이기 때문에 대괄호 안에는 'Previous'랑 'Next'가 쓰여 있고 소괄호 안에는 '@previous', '@next'라고 작성되어 있다.](image11.png){: w="700" h="400" }

![그 결과 랜더링한 화면에는 이전 페이지로 가는 previous 텍스트와 다음 페이지로 가는 next 텍스트가 나타난다. 주황색 텍스트로 나타나 있다.](image12.png){: w="700" h="400" }

더 많은 마크업 문법은 [코드로 간단하게 정리](https://gist.github.com/imjhk03/b2b039000b9afebc1ba7c13b251fccab)를 했고 더 깊이 있는 것은 [애플 문서](https://developer.apple.com/library/archive/documentation/Xcode/Reference/xcode_markup_formatting_ref/index.html#//apple_ref/doc/uid/TP40016497-CH2-SW1)나 검색해 보면 더 확인할 수 있다.

<br>
**참고**

[Multi Page Xcode Playground](https://www.youtube.com/watch?v=iy-sG8OGT9M)

[Markup Formatting Reference: Markup Overview](https://developer.apple.com/library/archive/documentation/Xcode/Reference/xcode_markup_formatting_ref/index.html)

[Swift Playground Markup Formatting Reference](https://gist.github.com/imjhk03/b2b039000b9afebc1ba7c13b251fccab)

