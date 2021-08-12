---
title: '분할정복알고리즘'
layout: single
author_profile: true
read_time: true
comments: true
share: true
related: true
categories:
- Algorism

# table of contents
toc: true # 오른쪽 부분에 목차를 자동 생성해준다.
toc_label: "table of content" # toc 이름 설정
toc_icon: "bars" # 아이콘 설정
toc_sticky: true # 마우스 스크롤과 함께 내려갈 것인지 설정
---

## 분할정복이란
⇒주어진 문제를 둘 이상의 부분 문제로 나눈 뒤 각 문제에 대한 답을 재귀 호출을 이용해 계산하고, 각 부분 문제의 답으로부터
전체 문제의 답을 계산한다.

### 재귀호출과 다른점
재귀호출 : 한 조각을 해결하고 나머지를 재귀 호출
<img src="/assets/images/algorism/divideAndConquer_2.jpg" width="40%" height="30%">

분할정복 : 거의 같은 크기의 부분 문제로 나누어 호출
<img src="/assets/images/algorism/divideAndConquer_1.jpg" width="40%" height="30%">

<p class="notice--info">
고로 문제의 크기가 매번 절반의 가깝게 줄어들기 때문에 기저 사례의 도달하기까지의 분할 횟수가 줄어든다.
</p>

### 분할정복의 구성 요소
1. 문제를 더 작은 문제로 분할하는 과정
2. 각 문제에 대해 구한 답을 원래 문제에 대한 답으로 병합하는 과정
3. 더이상 답을 분할하지 않고 곧장 풀 수 있는 매우 작은 문제(기저사례)

### 분할정복을 이용한 수열의 합

```java
int fastSum(int n){
    if(n == 1) return 1; //기저사례
    if(n % 2 == 1) return fastSum(n-1) + n; //홀수일 경우
    return 2 * fastSum(n/2) + (n/2) * (n/2); //문제 분할 및 병합
}
```

기존의 수열의 합을 구하는 방법은 n+(n-1)을 이용하기 때문에 n번의 시간복잡도를 가진다.  
분할정복을 이용할 경우 홀수가 아닌 경우 문제를 절반으로 나누기 때문에 nlog와 가까운  
시간복잡도를 가진다는 것을 알 수 있다.

<img src="/assets/images/algorism/divideAndConquer_3.jpg" width="100%" height="40%">
