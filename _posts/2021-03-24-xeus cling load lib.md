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
