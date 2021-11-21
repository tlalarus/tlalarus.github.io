---
title: Ethernet 케이블로 ssh 통신하기(Ubuntu)
categories: [dev]
comments: true
published: true
---
개발보드와 타겟보드 ssh통신을 통해서 좀더 편리한 개발환경 구성해보았습니다.   
Jetson nano 보드에 배포할 프로그램을 개발하려고 합니다. 배포환경과 개발환경이 꼭 동일할 필요는 없습니다.   
즉, 젯슨나노에 배포한다고 해서 젯슨나노에 에디터 설치해서 개발할 필요는 없다는 뜻입니다. 
## 원하는 Workflow
- 개발에 필요한 모든 툴이 갖춰진 내 laptop(__Client__)에서 코드를 작성한다.
- 작성한 코드를 타겟보드 환경에 맞게 cross-compile 한다.
- 빌드된 코드를 __타켓보드로 전송__ 한다.
- 타겟보드(__Server__)에서 빌드 결과물을 실행한다.   

## Prepare
서버쪽 필요한 패키지: ```ssh-server```   
클라이언트쪽 필요한 패키지: ```ssh-client``` (ubuntu에는 기본적으로 설치됨)
## SSH 접속설정
__Server__ 에서 tcp 포트 22번을 허용해준다.
```
$ sudo ufw allow 22/tcp
```
```/etc/ssh/sshd_config``` 파일을 아래와 같이 수정해서 root계정에 직접 로그인하는 것을 차단한다.
```
PermitRootLogin no
```
## SSH 접속
먼저, 서버에서 ssh접속을 열어준다.
```
# service sshd start
```
아래 명령어로 ssh서비스가 구동중인지 확인할 수 있다(Active 인지 확인).
```
# service sshd status
```
서버에서 랜선이 연결된 인터페이스(```eth0```)의 ip(```inet6```)를 확인한다.

```
$ ifconfig
```
클라이언트에서 ssh 접속을 시도한다.
```
ssh [USERNAME]@[IPADDRESS]
```
## 파일 동기화(rsync)
```
rsync -azvh source_dir destination_dir 
```
- -az : 압축한 데이터를 보낸다.(압축하지 않고 보낼시 권한정보가 보존되지 않을 수 있다.)
- -h : human-readable.
- -v : 자세한 정보를 출력한다.
- source_dir 에 slash가 붙으면 source_dir의 내용물만 전송되므로 주의할 것.   
```
rsync -azvh src_dir [USERNAME]@[IPADDRESS]:/home/[USERNAME]/
```