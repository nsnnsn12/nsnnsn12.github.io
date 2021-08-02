---
title: '한수'
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
<a href="https://www.acmicpc.net/problem/1065" target="_blank">https://www.acmicpc.net/problem/1065</a>

## 풀이
### 문제의 추상화
1. n이 주어졌을 때 1 <= x <= n x에 해당하는 모든 한수의 개수를 출력하라.
2. 한수란 양의 정수가 주어졌을 때 각 자리가 등차수열을 이룰 경우

### 문제의 계획
n번의 조각으로 나누어 n이 한수인지 확인하여 count를 올리고
n-1번부터 1까지의 한수의 갯수는 재귀로 넘김

### 코드
```java
public int apCount(int n) {
    if(n == 0) return 0;

    int rel = 0;
    rel = isAp(n)?1:0;
    rel += apCount(n-1);
    
    return rel;
}

public boolean isAp(int n) {
    if(n < 100) {
        return true;
    }
    
    boolean isTrue = true;
    int a = n / 100;
    int b = (n % 100) / 10;
    int c = (n % 100) % 10;
    
    if((a-b) != (b-c)) isTrue = false;
    
    return isTrue;
}
```