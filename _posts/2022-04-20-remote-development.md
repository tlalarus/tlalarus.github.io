---
title: pip ssl 에러 해결
categories: [log]
comments: true
---
```
'SSLError(SSLCertVerificationError(1, '[SSL: CERTIFICATE_VERIFY_FAILED] certificate verify failed: unable to get local issuer certificate (_ssl.c:1045)'))'   
```
pip install 을 실행했는데 위와 같은 에러가 발생했다. 알아보니 회사 컴퓨터나 사내망 같은 환경에서 작업할때 나타나는 현상이라고 한다. 사내망을 이용하면 pypl 같이 파이썬 패키지를 제공하는 사이트를 신뢰할 수 없는 사이트로 인식해서 발생하는 에러 같다. 
## 해결방법
```pip --trusted-host pypi.org --trusted-host files.pythonhosted.org install matplotlib```   
```pip install``` 의 ```--trusted-host``` 옵션으로 파이썬 패키지 서버 주소를 추가하면 문제는 해결된다. 하지만 매번 저 옵션을 직접 타이핑 하기에는 너무 길다.
## 더 편한 방법
우분투에서 alias를 사용하면 자주 사용하는 명령어를 단축키처럼 짧은 문자열로 실행할 수 있다.   
```
$ alias
``` 
위 커맨드를 실행하면 현재 등록된 단축명령어 목록을 확인할 수 있다.   

```
$ alias pip='pip --trusted-host pypi.org --trusted-host files.pythonhosted.org'
```
이렇게 등록하면 긴 옵션을 매번 타이핑하지 않아도 된다. 위처럼 설정한 명령어는 재부팅하면 리셋되기 때문에 ```.bashrc``` 파일 마지막에 위 명령을 추가해서 부팅시 단축문자열이 등록되도록 설정해두었다.
