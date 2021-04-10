---
title: cv::Mat format type
categories: [dev]
comments: true
---

## cv::Mat 의 자료형
opencv의 Mat 은 아래와 같은 자료형을 지원한다.   
* CV_8U : 8-bit unsigned integer: uchar ( 0...255 )
* CV_8S : 8-bit signed integer: schar ( -128... 127 )
* CV_16U : 16-bit unsigned integer: ushort ( 0...65535 )
* CV_16S : 16-bit signed integer: shor ( -32768...32767 )
* CV_32S : 32-bit signed integer: int ( -2147483648...2147483647 )
* CV_32F : 32-bt floating-point number: float ( -FLT_MAX...FLT_MAX, INF, NAN )
* CV_64F : 64-bit floating-point number: double ( -DBL_MAX...DBL_MAX, INF, NAN )
   

나는 이미지를 다루기 때문에 보통 CV_8U 형을 많이 사용하지만 행렬연산 목적으로 Mat을 사용할 때는 범위가 더 넓은 자료형을 쓰는 것이 좋을 것 같다. 또한, opencv의 영상처리 알고리즘들은 가끔 입력가능한 format type이 정해져 있기 때문에 사용하고자 하는 opencv 함수가 어떤 format을 지원하는지 확인하면서 사용하는 것이 좋다.
   
## format type 관련 에러
최근에 코드를 짜다가 Mat의 포맷관련한 문제를 몇가지 겪었다.   

### unsupported format
cv::filter2d 함수에서 아래와 같은 예외가 발생했다.
```
terminate called after throwing an instance of 'cv::Exception'
  what():  OpenCV(3.4.6) /opt/opencv/opencv-3.4.6/modules/imgproc/src/filter.simd.hpp:3172: error: (-213:The function/feature is not implemented) Unsupported combination of source format (=4), and destination format (=4) in function 'getLinearFilter'
```

확인해보니 filter2d 함수는 CV_8U, CV_16U, CV_32F 자료형인 입력만 지원한다고 한다. ~~opencv 공식문서에서 확인한 내용은 아니고 구글링으로 확인한 카더라임~~
   

### Print actual Mat element
코드를 짜다보면 내가 짠 코드가 맞는 것인지 결과 Mat을 통해 확인하고 싶을 때가 있다. 나는 직접 픽셀값을 출력해서 확인하는 편인데 ```std::cout``` 으로 출력하면 실제값과 다른 엉뚱한 숫자가 출력되는 것을 확인하였다.

```
case CV_8U:
{
    printf("%*u ", 3, mat.at<uchar>(r, c));
    break;
}
case CV_32S:
{
    printf("%*d ", 6, mat.at<int>(r, c));
    break;
}
```
위처럼 포맷마다 적당한 출력형식을 정해주어야 하는 것 같다.


