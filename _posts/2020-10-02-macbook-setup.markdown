---
title: '나의 새로운 Mac 설정 목록 (Setup my personal Mac)'
layout: post
tags: [workflow, macOS]
---

맥os를 사용한 지 6년이 되었고, 항상 새로운 맥북 혹은 초기화 후에는 나한테 맞는 설정을 한다. 새로 설정할 때마다 가끔 까먹을 때도 있어 글을 쓰게 되었다. 혹시나 이 글을 읽게 되었다면, 해당 내용 중에 유용한 거 있으면 적용해 보아도 좋을 것 같다.

1. 최신 소프트웨어로 업데이트하기
2. Apple ID
    - iCloud Drive - 데스크탑 및 문서 폴더 저장 비활성화
3. 화면 보호기 - 드리프트로 변경
4. Dock 설정
    - 크기 조절, 왼쪽, 자동으로 Dock 가리기와 보기, 최근 사용한 응용 프로그램 보기 비활성화
    - Dock 빈칸 생성하기 (터미널)

        ```
    defaults write com.apple.dock persistent-apps -array-add '{"tile-type"="spacer-tile";}'; killall Dock
        ```
    
    ![Dock with empty space](/assets/img/2020/10/02/image1.png){:height="1000px"}

5. Mission Control - 핫 코너
    - Spaces를 최근 사용 내역에 따라 자동으로 재정렬 체크 해제
    ![Mission Control 체크 리스트](/assets/img/2020/10/02/image5.png)
    - Launchpad, Mission Control, 데스크탑, 디스플레이 잠자기
    ![Mission Control 핫 코너](/assets/img/2020/10/02/image2.png)
6. 언어 및 지역 - 고급 - 날짜 설정
    - 짧게 - 20.06.01
7. 손쉬운 사용 - 확대/축소
    - 확대/축소하려면 스크롤 제스처를 다음 보조 키와 함께 사용 활성화
    ![단축키를 사용하여 확대하기](/assets/img/2020/10/02/image3.png)
8. 메뉴 막대에서 Bluetooth 보기 활성화
9. 키보드
    - 키보드 - Control Strip 사용자화
        - 잠자기
    - 단축키
        - 이전 입력 소스 선택&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;⌘스페이스
        - Spotlight 검색 보기&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;⌥스페이스
10. 트랙패드
    - 탭하여 클릭하기 활성화
    - 이동 속도 조절
11. 날짜 및 시간
    - 날짜 보기 활성화
12. 공유
    - 컴퓨터 이름 변경
        - 이렇게 하면 터미널에서 컴퓨터 이름이 나타난다
        ![터미널에 내 컴퓨터 이름 변경하기](/assets/img/2020/10/02/image4.png)
13. Finder
    - 보기
        - 경로, 상태 막대 보기
    - 환경설정
        - 폴더 우선 정렬 활성화
14. 배터리 퍼센트 표시하기
15. 필요한 프로그램들을 정리한 노트 기록하기
    - Chrome
    - Xcode
    - Apple Developer
    - Github Desktop
    - VSCode
    - Notion
    - Twitter
    - Dropbox
        - 배경화면 폴더 연동
    - Fig
    - Spotify
    - Spark Email App