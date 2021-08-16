---
title: '수열'
layout: single
author_profile: true
read_time: true
comments: true
share: true
related: true
categories:
- Implements

# table of contents
toc: true # 오른쪽 부분에 목차를 자동 생성해준다.
toc_label: "table of content" # toc 이름 설정
toc_icon: "bars" # 아이콘 설정
toc_sticky: true # 마우스 스크롤과 함께 내려갈 것인지 설정
---

## 문제
<a href="https://www.acmicpc.net/problem/2491" target="_blank">https://www.acmicpc.net/problem/2491</a>

## 풀이
### 문제의 추상화
N개의 숫자가 나열되어 있고 그 수열 안에서 연속해서  
커지거나(같은 것 포함) 작아지는(같은 것 포함) 수열 중 가장 길이가 긴 것을 찾아라

### 제한 사항
1 <= N <= 100,000

### 문제의 계획
0번째부터 n-1번까지 다 순회하며 연속되게 오름차순이나  
내림차순 카운트를 센다

### 코드
```java
public int result(int[] list) {
    int rel = 0;
    int n = list.length;
    int inc = 1;
    int dec = 1;
    for(int i = 1; i < n; i++) {			
        if(list[i-1] < list[i]) {
            ++inc;
            dec = 1;
        }else if(list[i-1] > list[i]) {
            ++dec;
            inc = 1;
        }else {
            ++inc;
            ++dec;
        }
        
        rel = Math.max(rel, dec);
        rel = Math.max(rel, inc);
    }
    return rel;
}
```