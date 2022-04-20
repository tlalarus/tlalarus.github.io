---
title: 원격서버의 컨테이너에서 Vscode로 개발하기
categories: [log]
---

요새 개발용으로 사용할 피씨를 새로 세팅하고 있다. 
개발환경을 배포환경과 최대한 비슷하게 세팅하기 위해 프로그램이 실제로 동작하는 원격서버와 IDE를 실행하는 개발서버를 나누려고 노력중이다.

vscode에서는 remote-ssh 로 원격서버의 코드를 편집할 수 있고 docker extension을 설치하면 컨테이너 내부의 코드를 편집할 수도 있다. 
그렇다면 원격서버의 컨테이너에도 접근할 수 있지 않을까?   

* Remote Development 확장을 설치한다(필요한 확장들이 줄줄이 설치됨).
* remote ssh 로 원격서버에 접속하면 remote explorer에서 원격서버에 attach된 컨테이너들을 확인할 수 있다.
* 원하는 컨테이너에 접속하면 끝.

컨테이너 목록에는 나오는데 아래 메시지가 나오면서 연결이 안되는 현상이 발생했는데 vscode를 최신버전으로 업데이트 하니까 해결되었다.
```
cannot attach to the container with name it no longer exists
```



참고한 사이트:   
https://code.visualstudio.com/docs/remote/containers