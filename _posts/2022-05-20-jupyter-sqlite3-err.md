---
title: 주피터 노트북 _sqlite3 module 에러 해결
categories: [log]
---

도커에 파이썬(3.8.13)을 설치하고 주피터 노트북을 실행하던 도중 다음 에러를 만났다.      

    ModuleNotFoundError: No module named '_sqlite3'

## 해결 방법
우선 ```libsqlite3-dev``` 패키지를 설치해야 한다.

    $ sudo apt-get install libsqlite3-dev

애초에 파이썬을 빌드할 때 저 패키지가 있었어야 하는 것 같은데 빠진 것 같다. 그래서 ```libsqlite3-dev``` 패키지를 설치하고 파이썬 소스를 다시 빌드해주었다.    
파이썬 코드와 빌드 가이드 링크 > (https://github.com/python/cpython)
