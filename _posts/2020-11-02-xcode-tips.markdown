---
title: Xcode 소소한 팁
layout: post
categories: [Tips, Xcode]
tags: xcode
---

Xcode 사용하면서 알게 된 소소한 팁들을 소개한다.

## 1. Annotation을 잘 활용하자
Xcode에서 TODO, MARK, FIXME Annotations이라는 것을 주석처럼 쓰면서 용도별로 사용할 수 있다.
1. TODO - 이름 그대로 작업하면서 해야할 일들을 쓴다.
2. MARK - 하나의 섹션 덩어리로 사용할 수 있다. 주로 영역 단위로 해당 영역이 무엇인지 표시한다.
3. FIXME - 나중에 수정이 필요한 부분을 표시한다.

![Xcode Annotiations, TODO, MARK, FIXME](/assets/img/2020/11/02/image1.png){:width="90%"}

Controller Outline에서는 아래와 같이 각 Annotation 아이콘이 달라 구별하면서 찾을 수 있다. 그 중에 MARK Annotation만 다른 annotation과 다르게 굵게 나타나고 위에 선이 나타나 조금 특이하다. Minimap에서는 영역별로 MARK 설명이 표시되어 찾기가 편하다.

![Controller Outline, which shows each annotation has a separate icon](/assets/img/2020/11/02/image2.png){:width="90%"}

![Minimap, which shows a block of section and shows what MARK annotation says](/assets/img/2020/11/02/image3.png){:width="25%"}

<br>

## 2. #warning을 잘 활용하자
위에서 설명한 Annotation도 잘 활용하면 좋지만, 주석처럼 보이게 되어 작업하다 보면 까먹을 수 있다. 이때 #warning 혹은 #error를 잘 활용하면 좋다. #warning(Message) 내용을 작성하면, 빌드하면서 Issues Navigator에 노란색 warning 부분에 해당 내용이 나타난다. Warning을 하나씩 제거하면서 작업하면 어느새 TODO 리스트처럼 활용할 수 있다. #error는 warning과 비슷하지만 빌드가 안되는 차이가 있다.

![#warning, write a message and shows the message at the Issue Navigator](/assets/img/2020/11/02/image4.png){:width="90%"}

<br>

## 3. 검색한 결과들 중 필요없는 파일 숨기기
Find Navigator에서 특정 단어 혹은 파일 이름을 사용한 부분들을 검색하면 생각보다 많은 검색 결과들이 나타날 수 있다. 검색 결과들을 하나씩 보고 파일을 접어서 사용했었는데, delete (⌫) 키를 사용하면 to-do 리스트 처럼 활용할 수 있다. 파일 혹은 해당 영역을 더 이상 보지 않는다면, delete 키를 사용하여 더 이상 나타나지 않게 할 수 있다.

![GIF of using delete key for disappearing search result at Find Navigator](/assets/img/2020/11/02/image5.GIF)