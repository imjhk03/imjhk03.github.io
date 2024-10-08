---
title: Xcode 시뮬레이터 동영상 녹화하는 방법
layout: post
categories: [Tips, Xcode]
tags: [xcode]
---

시뮬레이터로 앱 테스트하다 보면 스크린샷을 찍어서 사진으로 사용할 수 있다. 하지만 동영상으로 녹화해서 공유하고 싶은 상황이 발생할 수 있는데, 예전에는 Quick Player 앱을 사용하는 등 다른 방법으로 녹화해서 하는 방법이 있다. 하지만 Xcode 시뮬레이터에서 직접 동영상을 녹화하는 방법이 있다.

> Xcode 12.5 이상부터 가능한 것으로 보입니다.

# 1. 동영상 녹화하기

**Simulator** 앱에서 **File > Record Screen(⌘ + R)**을 누르면 바로 시뮬레이터 동영상 녹화가 시작한다.

![시뮬레이터 앱에서 File > Record Screen 메뉴](/assets/img/2021/09/02/image1.png)

시뮬레이터에서 녹화가 시작하면 아래와 같이 빨간 녹화 버튼이 나타난다. 녹화를 끝내고 싶다면 해당 빨간 녹화 버튼을 누르면 된다.

![시뮬레이터 상단에 나타나는 녹화 버튼](/assets/img/2021/09/02/image2.png)

# 2. 녹화한 동영상 저장하기

녹화를 끝내면 시뮬레이터 옆에 아주 작게 녹화한 동영상이 재생된다. 기본적으로 데스크탑 폴더에 mp4 형식으로 저장된다. 여기서 오른쪽 버튼을 누르면 어디에 저장할지, gif 형식으로 저장할지 선택할 수 있다.

![녹화한 동영상은 시뮬레이터 옆에 작게 나타난다](/assets/img/2021/09/02/image3.png)

# 환경 설정

**Simulator menu > Preferences** 메뉴를 선택하면 시뮬레이터에 대한 설정을 할 수 있다. 예를 들어서 Single Touch 하는 것을 보여주거나 GIF 사이즈를 조절할 수 있다.

![시뮬레이터의 설정을 할 수 있는 Preference 창](/assets/img/2021/09/02/image4.png)

# 결론

며칠 전에 시뮬레이터 동영상을 녹화하는 방법을 최근에서야 알게 된 사람들을 보면서, 관련하여 글을 작성해보면 좋겠다고 생각해 글을 작성해 보았다. 앞으로도 종종 잘 모르는 팁들이 있으면 블로그 글을 작성하여 공유할 예정이다.