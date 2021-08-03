---
title: '연속부분최대곱'
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
<a href="https://www.acmicpc.net/problem/1543" target="_blank">https://www.acmicpc.net/problem/1543</a>

## 풀이
### 문제의 추상화
1. n개의 실수가 있을 때 연속된 수들의 곱에 대한 최댓값을 찾아라

### 제한 사항
1 <= n <= 10000
0.0 <= x <= 9.9

### 문제의 계획


### 코드
```java
public double[] list;
public double result(int n, int depth) {
    if(n == depth) return 0;
    
    double rel = list[depth];
    double count = list[depth];
    for(int i = depth+1; i < n; i++) {
        count *= list[i];
        rel = Math.max(rel, count);
    }
    
    rel = Math.max(rel, result(n, depth+1));
    return rel;
}
	
```