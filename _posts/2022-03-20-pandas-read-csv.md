---
title: Pandas parquet로 데이터 빨리 읽기
categories: [dev]
---
머신러닝에서 데이터를 다룰 때, 엑셀 호환성때문에 ```.csv```를 많이 사용하고 있다. 약 3GB 크기의 csv 파일을 pandas로 불러오는데 ```pd.read_csv()``` 수행 중 실행이 종료되는 문제가 발생했다.

# read_csv() default로  실행
일반적으로 데이터 column마다 다른 자료형을 사용한다. 별도의 옵션을 주지 않는 한 Pandas ```read_csv()```는 데이터 column의 자료형을 추론하게 된다. 이 추론 과정은 생각보다 오래 걸린다(체감상 11column 1GB 데이터를 읽는데 1분걸리는듯..). 시간과 별개로 수행중 갑자기 종료되는 것이 문제라고 생각한다.   
   
1GB보다 작은 데이터 로드시 정상적으로 동작하는 걸로 보아 데이터 용량이 클수록 RAM을 많이 사용하는 것과 관련된 문제로 파악된다. pandas read_csv 를 구글링 해보면 수행시간을 단축하기 위한 옵션 사용법에 관한 글을 많이 찾아볼 수 있다.

# usecols, dtype, converters 옵션 지정
Pandas가 데이터형을 추론하는 시간을 줄이기 위해 [```read_csv()```](https://pandas.pydata.org/docs/reference/api/pandas.read_csv.html)의 옵션을 사용해보았다.
* ```usecols``` : 전체 column 중 특정 column만 로드할 수 있다.
* ```dtype``` : column의 데이터형을 직접 지정한다.
* ```converters``` : 특정 column에 대해 지정한 함수를 사용해서 변환작업을 수행한다(이상치 처리시 사용하면 편리).   

사용해본 결과, warning 메세지는 줄어들었지만 여전히 종료되는 문제가 간헐적으로 발생했다.

# Parquet
Parquet 형식은 [Snappy](https://ko.wikipedia.org/wiki/%EC%8A%A4%EB%82%B4%ED%94%BC_(%EC%95%95%EC%B6%95))라는 압축 방식을 사용하는데 이 방식은 구글에서 개발한 대용량 처리에 적합한 압축방식이라고 한다. 압축률 보다는 속도에 초점을 맞춘 방식인 것 같다.
## dependencies
파이썬에서 parquet을 사용하기 위해서는 ```libsnappy-dev fastparquet pyarrow``` 패키지가 설치되어 있어야 한다.
## 사용법
판다스에서도 parquet에 대한 IO를 지원하고 있다.   
* [```pandas.DataFrame.to_parquet```](https://pandas-docs.github.io/pandas-docs-travis/reference/api/pandas.DataFrame.to_parquet.html)
* [```pandas.read_parquet```](https://pandas-docs.github.io/pandas-docs-travis/reference/api/pandas.read_parquet.html#pandas.read_parquet)
## Using C++
나는 데이터 생성 프로그램을 c++로 작성하고 있어서 c++로 parquet형식을 저장하는 방법에 대해서도 찾아보았다. Apache에서 parquet IO를 위해 제공하는 [parquet-cpp](https://github.com/apache/parquet-cpp)를 사용해보기로 했다.


