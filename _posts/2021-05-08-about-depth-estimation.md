---
title: Depth estimation 관련 용어 및 개념 정리
categories: [dev]
comments: true
---

싱글 영상을 이용한 depth estimation을 공부해보려고 합니다. 헷갈리는 개념들이 있어서 정리해보았습니다.
   

   
# Depth estimation 이란?
Depth는 카메라에서부터 영상의 어느 (u, v) 좌표지점까지의 실제 거리를 측정하는 것과 깊은 관련이 있습니다. 우리가 흔히 말하는 이미지는 3D 포인트들이 2D로 mapping되어 depth정보가 손실됩니다. 

2D를 3D로 _inverse_ 하고 싶다면? 이것은 2D 이미지만 가지고 scene을 복원하겠다는 의미입니다. depth estimation에 관한 많은 연구들이 이루어지고 있습니다. 많이 연구되었고 가장 성공적인 방법은 stereo vision을 이용하는 기술입니다. 그리고 deep learning을 이용한 기술도 엄청난 성능을 보이고 있습니다.
   
## Back-projection
Depth map과 Intrinsic parameter 및 extrinsin parameter 을 알고 있다고 가정한다면 3D 가 2D 로 변환되는 식을 역산하여 3D좌표를 구할 수 있습니다.

# 관련 개념
### SLAM

### Point cloud

# 관련 Keyword
### ORB-SLAM
### ARCore

# 공부할 내용
https://taeyoung96.github.io/paper_review/BTS/