---
title: Camera Coordinate System
categories: [dev]
comments: true
---
캘리브레이션에서 일반적으로 다루는 좌표계에는 카메라 좌표계, 이미지 좌표계, 월드 좌표계가 있다. 캘리브레이션에서 camera pose는 중요한 정보인데, 카메라 좌표계와 월드 좌표계 사이의 관계를 통해서 camera pose를 구하는 방법을 공부하면서 정리해 보았다.

<!-- 좌표계 그림 -->
![](https://learnopencv.com/wp-content/uploads/2020/02/world-camera-image-coordinates.png, "좌표계")

## Camera pose
Extrinsic parameter 를 알고 있다면 월드좌표계 상의 카메라 위치와 각도를 계산할 수 있다. 어떤 임의의 3D 월드 좌표가 카메라 내부의 이미지 평면에 어떻게 변환되는지 나타낸 행렬이 Extrinc parameter 이기 때문이다. 

월드좌표와 카메라좌표 간에는 아래와 같은 관계가 성립된다. 

<!-- 월드좌표 = 카메라좌표 * 변환행렬  -->
![](../assets/img/posts/2024-01-26-camera-pose_01.png, "camera-world 변환")
[Xc, Yc, Zc]T = R^-1 ([X, Y, Z]T - [X1, Y1, Z1]) (X1, Y1, Z1 은 월드좌표계 상의 카메라 원점의 위치)

카메라의 위치좌표는 카메라 원점의 월드좌표를 의미하며 이를 구하는 간단한 방법은 위 식을 적용하여 아래와 같이 계산할 수 있다.
<!-- 카메라 위치 구하는 식 -->
![](../assets/img/posts/2024-01-26-camera-pose_02.png, "camera position")


## Camera angle
카메라 광학축을 기준으로 pan, tilt, roll은 다음과 같이 정의된다.
* Pan  : 카메라가 수평방향(좌우)로 회전된 각도. (yaw)
* Tilt : 카메라가 수직방향(상하)로 회전된 각도. (pitch)
* Roll : 카메라의 광학축을 기준으로 회전한 각도. (roll)

아래처럼 pan, tilt, roll은 월드좌표계의 X, Y, Z 축의 회전과 관련이 있다. 사용자가 월드좌표계를 어떻게 설정하는지에 따라 pan, tilt, roll의 정의가 달라진다.
<!-- 카메라 팬틸트롤 정의하는 좌표계 그리기 -->
![](../assets/img/posts/2024-01-26-camera-pose_03.png, "camera angle")

위의 경우, ```pan```은 __월드X축__ 기준 회전 각도, `tilt` 는 __월드Y축__ 기준 회전 각도, `roll` 은 __월드Z축__ 기준 회전 각도로 정의 할 수 있을 것이다. 



회전 변환 행렬 R은 각 축의 회전행렬의 곱으로 표현할 수 있다. Opencv에서는 회전변환행렬R에 평행이동변환행렬 T를 추가한 Projection Matrix에서 거꾸로 pan, tilt, roll을 분해하는 함수 [``` cv::decomposeProjectionMatrix ```](https://docs.opencv.org/3.4/d9/d0c/group__calib3d.html#gaaae5a7899faa1ffdf268cd9088940248) 를 제공한다. 이 함수는 Z,Y,X 축 순서대로 회전했다는 가정이 존재한다(통상적으로 Z,Y,X 순서를 따른다고 함).


## 참고 
* https://darkpgmr.tistory.com/122
* https://tkayyoo.tistory.com/19
* https://learnopencv.com/geometry-of-image-formation/