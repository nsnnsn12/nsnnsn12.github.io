---
title: '적어도 대부분의 배수'
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
<a href="https://www.acmicpc.net/problem/1145" target="_blank">https://www.acmicpc.net/problem/1145</a>

## 풀이
### 문제의 추상화
1. 5개의 자연수가 존재할 때 5개 중 3개 이상의 배수가 되는 최소 값을 구하라

### 문제의 계획
1부터 시작하여 적어도 3개 이상의 배수가 되는지 확인 되지 않는다면 1씩 더한다.

### 코드
```java
public final int N = 5;
public int result(int[] list, int target) {
    int count = 0;;
    for(int i = 0; i < N; i++) {
        if(target % list[i] == 0) count++;
    }
    
    if(count >= 3) return target;
    
    int rel = result(list, target+1);
    
    return rel;
}
```