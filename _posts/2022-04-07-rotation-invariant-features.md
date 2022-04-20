---
title : Rotation invariant features
categories : [study]
---
회전에 불변한 영상특징을 조사해 보았다.

* feature 종류에는 local features, global features가 있다.
* 회전 불변 특성을 갖기 위해서는 pose normalization 이 필요하다.(예를들어 sift는 dominant orientation을 찾아서 작업을 한다.)
* 기존 hog feature는 완전한 회전불변 아님: 고정 좌표 시스템, 객체를 cell에 표현하는 방식의 샘플링은 rotation invariant 하지 않음   

자료를 찾아보다가 아래 개념에 대해 배우게 되었다.
1. Polar histogram

2. Rotation invariant histogram
* 2d, 3d rotation에 대해 다룸.
* 퓨리에변환을 사용해서 histogram을 생성한다.(polar and spherical coordinates 이용)
* learning 방식의 어려움 : 3d rotation은 회전축이 3개이므로(오일러회전) 샘플링할 데이터가 너무 많아진다.

퓨리에분석으로 회전불변 특징을 생성한 논문도 있었다.
* 퓨리에분석은 이미지 스펙트럼의 주파수영역을 다룰때 사용된다. 
* 퓨리에분석으로 만든 회전불변 descriptor는 더 나은 성능을 보인다.
* 퓨리에 기반 회전불변성을 생성하기 위해서: 3D HOGfeature에 spherical tensor algebra를 적용한다.
* 나중에 찾아볼 키워드 : spherical harmonics

나는 shape context에 관한 피쳐를 구현했다.
* shape context는 컨투어 픽셀의 상대적인 위치 분포를 히스토그램으로 표현한다. 
* basic idea : n개의 포인트를 컨투어에서 뽑는다. 각 포인트 파이마다 나머지 n-1개의 상대 좌표히스토그램.
히스토그램 bin은 log-polar 공간에 표현한다. 
* shape context에 회전불변을 부여하기 위한 방법 : 각 피쳐포인트의 normal vector에 대한 상대각도를 측정한다.


* 폴라 히스토그램 비교
    * 일반적인 비교 method : Kolmogorov-Smirnov test , chi-Square distance
    * Chi-Square distance : weighted average를 제공한다.