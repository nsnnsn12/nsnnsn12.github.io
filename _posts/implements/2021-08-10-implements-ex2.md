---
title: '수 이어 쓰기 1'
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
<a href="https://www.acmicpc.net/problem/1748" target="_blank">https://www.acmicpc.net/problem/1748</a>

## 풀이
### 문제의 추상화
1부터 N까지의 수를 이어서 쓸 때 자릿수의 개수는?

### 제한 사항
1 ≤ N ≤ 100,000,000  
시간제한 = 0.15  

### 문제의 계획
1-9 = 9 = 1*9 (1자리)  
10-99 = 90 = 10*9 (2자리)  
100-999 = 900 = 100*9 (3자리)  
각 자릿수마다 나오는 갯수를 더해주어야 한다.

### 코드
```java
public int result(int n) {
    int rel = 0;
    //십의 자리
    int size = 1;
    //자릿수
    int count = 1;
    while(true) {
        if((size * 10) - 1 < n) {
            rel += (size * 9) * count;
            size *= 10;
            count++;
        }else {
            rel += (n - (size - 1)) * count;
            break;
        }
    }
    return rel;
}
```