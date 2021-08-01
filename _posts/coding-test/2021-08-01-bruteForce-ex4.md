---
title: '초6 수학'
layout: single
author_profile: true
read_time: true
comments: true
share: true
related: true
categories:
- CodingTest

# table of contents
toc: true # 오른쪽 부분에 목차를 자동 생성해준다.
toc_label: "table of content" # toc 이름 설정
toc_icon: "bars" # 아이콘 설정
toc_sticky: true # 마우스 스크롤과 함께 내려갈 것인지 설정
---

## 문제
<a href="https://www.acmicpc.net/problem/2702" target="_blank">https://www.acmicpc.net/problem/2702</a>

## 풀이
### 문제의 추상화
1. 두 정수 a,b에 대한 최소 공배수 최대 공약수를 구하라

### 문제의 계획
공배수 : 1부터 곱하여 a가 클 경우 b의 곱을 더하고 b가 클 경우 a의 곱을 더하는 작업을 두 개가 같아질 때까지 반복한다.

공약수 : a,b 둘 중에 하나의 수에서 1이 될 때까지 차감하며 공약수가 나올 때 까지 반복한다.

### 코드
```java
public int lcm(int a, int b, int am, int bm) {
    if((a * am) == (b * bm)) return a * am;
    int result = 0;
    if((a * am) > (b * bm)) {
        result = lcm(a, b, am, bm+1);
    }else {
        result = lcm(a, b, am+1, bm);
    }

    return result;
}

public int gcd(int a, int b, int count) {
    if((a % count == 0) && (b % count == 0)) return count;
    int result = 0;
    result = gcd(a, b, count -1);
    return result;
}
```