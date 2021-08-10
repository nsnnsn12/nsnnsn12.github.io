---
title: '문자열'
layout: single
author_profile: true
read_time: true
comments: true
share: true
related: true
categories:
- BruteForce

# table of contents
toc: true # 오른쪽 부분에 목차를 자동 생성해준다.
toc_label: "table of content" # toc 이름 설정
toc_icon: "bars" # 아이콘 설정
toc_sticky: true # 마우스 스크롤과 함께 내려갈 것인지 설정
---

## 문제
<a href="https://www.acmicpc.net/problem/1120" target="_blank">https://www.acmicpc.net/problem/1120</a>

## 풀이
### 문제의 추상화
1. 문자열 b와 문자열 b보다 길이가 작거나 같은 문자열 a가 주어진다.
2. 문자열 a가 b와 길이가 같아질 때까지 a의 앞 혹은 뒤에 아무 알파벳을 추가할 수 있다.
3. 이 때 a와b의 길이를 같게 만들면서 두 문자열의 최소 차이값을 출력하라.
4. 문자열 간의 차이: a[i] != b[i]

### 문제의 계획
결국 문자열 a에 아무 알파벳이나 앞뒤로 붙힐 수 있다는 것은
추가되는 알파벳은 b문자열과 동일하게 만들 수 있다는 뜻이므로
처음 문자열 a가 b와 최소한으로 차이가 나게끔 매칭시키면 됨.

### 코드
```java
public final int INF = 51;
public int nonEqualsCount(char[] a, char[] b, int start) {
    if((a.length + start) > b.length) return INF;
    
    int rel = 0;
    for(int i = 0; i < a.length; i++) {
        if(a[i] != b[i+start]) rel++;
    }
    
    rel = Math.min(rel, nonEqualsCount(a, b, start+1));
    
    return rel;
}
```