---
title: docker 컨테이너에서 usb 카메라 SDK 설치
categories: [dev]
comments: true
---

새로운 프로젝트에서 사용하는 Jetson 보드에 카메라 동작 기능을 구현하고 있습니다.   
아래의 이유로 docker 이미지를 생성하기로 했습니다.
1. 보드에서 직접 빌드하는 방식은 느려서 불편하다.
2. 개발환경을 구체적으로 명시하고 싶어서.
3. 내 PC에서 개발하고 있는 다른 프로젝트들과 개발환경이 꼬일까봐 독립된 환경을 구성하고 싶어서.
   
 
이제 막 docker를 공부하고 있어서 진행이 더디지만 Dockerfile에 카메라 제조사의 홈페이지로부터 SDK 패키지를 다운로드 받는 것 까지는 성공했습니다.
SDK의 install 을 실행하니 설치 도중   
``` cp: cannot create regular file 'etc/udev/rules.d/': No such file or directory ``` 메세지가 나오면서 종료되었습니다.
젯슨 보드에 직접 설치하는건 잘 되는데... 컨테이너에서 usb 장치에 접근하기 위한 어떤 모듈이 존재하지 않는 상태가 원인인 것 같은 느낌이 듭니다.
  

아래는 해결방법인 듯 한 참고 페이지   
https://forums.balena.io/t/docker-container-cannot-access-dynamically-plugged-usb-devices/4277   
https://www.losant.com/blog/how-to-access-serial-devices-in-docker   

아직 시도는 안해봤지만 ``` etc/udev/rules.d ``` 파일을 직접 생성해주는 방법인 것 같습니다. 시도해본 결과는 아래에 추가할 예정!

05-12 추가 : udev 패키지를 설치해서 해결했습니다.
``` apt-get install udev ```