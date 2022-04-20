---
title: Docker 컨테이너에서 호스트의 파일에 접근하기
categories: [log]
comments: true
---

도커에서 volumes을 사용하면 호스트의 파일에 접근할 수 있다. 데이터가 모두 컨테이너 내부에 존재한다면 이미지 용량이 어마어마해 질 수 있다.
그래서 용량이 큰 데이터들은 모두 호스트에 두고 컨테이너 내부 프로그램이 필요할 때 호스트 파일에 접근할 수 있도록 구성했다.

도커에서는 이를 위한 기능을 ```--mount``` 옵션으로 제공한다(```-v``` 옵션도 있는데 mount옵션이 더 직관적이어서 사용하기 편하다고 함.)
도커에서는 volume 이라는 기능으로 마운트할 공간을 관리한다.

## 볼륨 생성
```
$ docker volume create my-vol
# my-vol 이라는 볼륨이 생성됨
```

## 볼륨 목록 확인
```
$ docker volume ls
```
## 개별 볼륨 설정 확인
```
$ docker volume inspect my-vol
# "Mountpoint"가 컨테이너 내부에 연결할 경로를 의미함.
```

## 컨테이너에 실행시 볼륨 연결하기
```
$ docker run -it --mount source=my-vol,target=/app ubuntu:18.04
```
위 명령이 정상적으로 실행되려면 컨테이너 내부에 ```/app``` 디렉토리가 존재해야 한다.
```
$ docker inspect <container-name>
```
위 명령을 실행하면 컨테이너 정보를 확인할 수 있는데 "Mounts" 에서 연결된 상태를 확인할 수 있다.

참고한 사이트:   
https://docs.docker.com/storage/volumes/#choose-the--v-or---mount-flag
https://blog.naver.com/PostView.naver?blogId=alice_k106&logNo=220421203146&redirect=Dlog&widgetTypeCall=true&directAccess=false