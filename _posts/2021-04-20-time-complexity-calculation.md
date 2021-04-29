---
title: 시간복잡도 계산하기
categories: [dev]
comments: true
---

최근에 매칭 알고리즘 코드를 짰는데 테스트해보니 수행시간이 너무 오래 걸려서 시간복잡도를 한번 계산해보았다.

pseudo code 는 아래와 같다.

```
n = t시점의 매칭점 개수
m = t+1시점의 매칭점 개수

for(k=3, k=Min(n,m)) // k는 Min(n,m) 까지 도달함.
{
    n_ = Permutation(n, k) // nPk 여기 시간복잡도는 일단 보류.
    m_ = Permutation(m, k) // mPk

    for(i=0, i=n_)
    {
        for(j=0, j=m_)
        {
            // matching1 함수는 k값에 영향을 받는다.
            err1 = matching1(k) {k+k+k}; 

            if(err1 >= THRESH)
                continue;
            
            // matching2 함수도 k값에 영향을 받는다.
            err2 = matching2(k) {k+k+k};

        }
    }

}
```

첫번째 for 문의 시간복잡도를 n,m에 대해 나타내면 ``` Min(n,m)-3+1 ``` 이다.
n_, m_은 ``` n', m' ``` 을 의미하며 이를 n,m에 대해 나타내면
``` n'm' = P(n,k)P(m,k) ```
와 같다. 이는 ``` 2k-2  ``` 차수를 가진다. ~~순열을 쓴거부터 이미 여기부터 망한것 같다ㅜㅠ~~
   

``` 3 <= k <= Min(n,m) ``` 을 이용해서 ``` 2k-2 ``` 의 최대값을 구하려고 한다.
``` Min(n,m) ``` 은 어차피 n이나 m 둘중 하나의 값이므로 n이 항상 작다고 가정하고 ``` Min(n,m) = n ``` 으로 치환하면 2k-2 = 2n-2 이다.
   

``` match_node(), match_edge ``` 함수의 시간복잡도는 각 3k이고 if문을 고려해서 총 과정의 시간복잡도는 6k로 가정하고 6k=6n 으로 계산한다.

첫번째 ``` for(k=3, k=Min(n,m)) ``` 반복문 시간복잡도는 n,
두번째 ``` for(i=0, i=n_) for(j=0, j=m_) ```의 시간복잡도는 2n-2
세번째 for문 안의 시간복잡도는 6n 이므로
n * (2n-2) * 6n = 13n^3-13n^2 이다.
Big O 로 표기하면 O(n^3) 이 된다. 최적화가 필요할 듯 싶다.
   
   
   
***
**2021-04-30 변경사항 기록**   
   

알고리즘을 내용적으로 분석해보니 쓸데없이 Permutation을 사용하고 있었다. 위 코드에서 n_을 Permutation이 아닌 Combination으로 변경했다.   
속도가 많이 줄어든 것을 확인함. 1초 넘게 걸리던게 100ms 안에 다 끝나버렸다.


   

