---
title: ethernet 케이블로 ssh 통신하기(Ubuntu)
categories: [dev]
published: false
---

Jetson nano를 타겟으로 하는 프로그램 개발 중 편하게 작업하기 위한 개발환경 구성에 대해 작성합니다.   
Workflow: client보드에서는 코드 작성 및 빌드(cross-compile)작업을 하고 빌드 결과물을 server보드로 전송해서 프로그램 실행.   
# Prepare client
__필요한 패키지들__   
```ifplugd openssh inxi rsync```   
   
__이더넷 인터페이스 이름 확인__   
```
$ inxi -i
Network:   Device-1: Intel Wireless 8265 / 8275 driver: iwlwifi 
           IF: wlp1s0 state: up mac: f8:63:3f:07:af:a3 
  
CLIENT_INTERFACE=wlp1s0
```
__ifplugd 실행__   
이더넷 인터페이스 이름으로 ```ifplugd``` 실행.   
```systemctl start ifplugd@wlpls0``` 또는   
```systemctl start ifplugd@${CLIENT_INTERFACE}```   
# Prepare server
__필요한 패키지들__   
```ifplugd openssh-server inxi```   
__이더넷 인터페이스 이름 확인__   
```
$ inxi -i
```

__ifplugd 실행__   
이더넷 인터페이스 이름으로 ```ifplugd``` 실행.   
```systemctl start ifplugd@wlpls0```

__ssh 실행__   
```systemctl start sshd```

__이더넷 케이블 연결__   
이더넷 케이블을 연결하고 ``` ifplugstatus -v ``` 명령으로 연결을 확인한다.   

__Server ip 확인__   
Server 이더넷 인터페이스의 IPv6 주소를 확인한다.
```
$ ifconfig | grep inet6 | head -n 1 | awk '{print $2}'
$ inxi -ix
```
```
SERVER_USER_NAME=
SERVER_IPV6="ip-v6 value"
```
__Client에서 Server로 ssh 실행__
```
ssh ${SERVER_USER_NAME}@${SERVER_IPV6}%${CLIENT_INTERFACE}
```


