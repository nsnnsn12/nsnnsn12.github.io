---
title: '문서 검색'
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
1. 문자열 a와 b가 주어질 때 문자열 b가 겹치지 않게 문자열 a에서 나오는 최대 갯수를 출력하라

### 제한 사항
1 <= a <= 2500
1 <= b <= 50
알파벳 소문자와 공백으로 이루어져 있다.

### 문제의 계획
문자열 a의 대해 b를 다 대입하는 최대 경우의 수는 2500 * 50이므로 완전탐색 가능. 
겹치지 않는 내에서 최대한의 중복을 발견하는 것임으로 중복 발견 시 발견한 index로
부터 단어수만큼 건너뛰어 검색을 시작한다.

### 코드
```java
public int getDoubleCheckCount(char[] a, char[] b, int start) {
    if((start + b.length) > a.length) return 0;
    
    boolean isDouble = true;
    for(int i = 0; i < b.length; i++) {
        if(b[i] != a[i+start]) {
            isDouble = false;
            break;
        }
    }
    int rel = 0;
    if(isDouble) {
        rel = 1 + getDoubleCheckCount(a, b, start + b.length);
    }else {
        rel = getDoubleCheckCount(a, b, start + 1);
    }
    return rel;
}
```