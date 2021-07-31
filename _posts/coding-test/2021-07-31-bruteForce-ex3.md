---
title: '슈퍼마리오'
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
<a href="https://www.acmicpc.net/problem/2851" target="_blank">https://www.acmicpc.net/problem/2851</a>

## 풀이
### 문제의 추상화
1. int 배열이 주어졌을 때 배열의 순서대로 값을 더하여 최대한 100과 가까운 숫자를 출력하라.
2. 가까운 수가 2개라면 큰 수를 출력한다.
3. 순서대로 값을 더하며 중간의 값을 건너뛸 수 없다.

### 문제의 계획
각 위치까지의 더한 값을 리스트에 저장하고 100과 가장 가까운 수를 출력한다.

### 코드
```java
public int result(int[] list) {
    for(int i = 1; i < 10; i++) {
        list[i] += list[i-1];
    }
    
    int result = 2000;
    int index = 0;
    for(int i = 9; i >= 0; i--) {
        int now = 100 - list[i];
        if(now < 0) now *= -1;
        
        if(result > now) {
            result = now;
            index = i;
        }
    }

    return list[index];
}
```