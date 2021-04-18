---
title: xeus cling 라이브러리 설정
categories: [dev]
comments: true
---

xeus cling 환경은 커널과 통하는 프로토콜을 바꿔준다. 그래서 cpp를 python처럼 주피터 노트북에서 interactive한 방식으로 사용할 수 있다.   
xeus는 주피터 커널 프로토콜을 cpp로 구현한 라이브러리라고 한다. 여하튼 나는 아나콘다에 xeus cling 환경을 세팅해서 개발하고 있는데 간단한 코드 테스트할 때 매우 유용한 것 같다.   
다만, cpp는 변수나 함수 선언 위치나 순서를 지켜줘야 하기 때문에 아무래도 다른 점이 있긴 하다.
python에서 라이브러리 import 할때는 셀 하나에 여러 모듈을 import 하곤 했어서 cpp에서도 한 셀에 include랑 load_lib을 다 때려넣고 실행했더니 커널이 죽는 현상이 있었다.   

![](https://user-images.githubusercontent.com/78094857/112333756-35f89f00-8cfe-11eb-8a88-afddab84dd52.png)
이런 에러가 나거나 커널이 죽어버렸다.   
   
   
코드 한줄을 한셀에 작성하니 문제가 해결 되었다.   
![](https://user-images.githubusercontent.com/78094857/112333744-33964500-8cfe-11eb-94fc-a59c26c5f281.png )
많은 라이브러리들을 일일이 쓰기엔 많아서 "xeus-cling-include.h" 헤더파일을 하나 생성하고 이 파일에서 필요한 라이브러리를 불러오도록 했다.   
   
   
2021.04.18 추가 기록.
imshow 함수가 동작은 하지만 이미지가 노트북에 보이지 않는 문제가 있었다.
노트북에서는 이미지를 url기반으로 표시하기 때문에 cv::imshow함수의 인자를 이에 맞게 재정의해주어야 한다고 한다.
먼저 세팅한 환경을 다시 살펴 보았다. 내가 깃허브에서 가져온 환경은 root기반으로 xeus-cling이 세팅이 되어야 하는데 나는 아나콘다 기반으로 설치를 했기 때문에 문제가 있는 듯 하다.

제공되는 install.py 내용은 `/usr/local/include/jupyter`에 `jupyter_opencv_include.h`파일을 생성하고 이 파일에 cv::imshow를 재정의한 코드를 붙여넣기한다.
그리고 파일명에 `libopencv`가 포함된 파일을 스캔해서 cling 라이브러리 경로에 추가해준다.

내 상황을 정리하자면
1. 주피터노트북이 실행되는것은 anaconda 의 가상환경 env에서 실행됨.
2. `/usr/local/lib  /usr/local/include `에 존재하는 opencv 라이브러리를 사용해서 `imshow`를 재정의한 `jupyter_opencv_include.h`를 `/usr/local/include/jupyter`에 생성.
3. 주피터노트북에서 `jupyter_opencv_include.h`를 읽어야 함.

