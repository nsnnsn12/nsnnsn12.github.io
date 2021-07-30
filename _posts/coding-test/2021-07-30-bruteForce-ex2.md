---
title: '설탕배달'
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
<a href="https://www.acmicpc.net/problem/2839" target="_blank">https://www.acmicpc.net/problem/2839</a>

## 시행착오
5000이 되기까지 3과 5를 고르는 모든 경우의 수는 대략 2의 1667승으로 완전 탐색으로 계산할 수 없었다.

## 제대로 된 풀이
### 문제의 목적
1. n이 주어졌을 때 3과 5를 이용하여 최소한의 몫을 구하라
2. 3과 5를 이용하여 n이 나누어 떨어지지 않는 경우 -1을 리턴

### 문제의 제약사항
1. n의 범위 3 ≤ N ≤ 5000
2. 제한시간 1초

### 문제의 계획
5000이 되기까지 3과 5를 고르는 모든 경우의 수는 대략 2의 1667승으로 완전 탐색으로 계산할 수 없다. 하지만 3과 5를 이용하여 숫자를 만드는데 많은 중복이 존재하고 우리가 원하는 결과는 최소한의 연산 횟수이므로 연산횟수를 배열을 이용하여 저장하고 최소 연산 횟수일 경우에만 재귀를 반복한다.

### 풀이
```java
public int[] memo = new int[5010];
public final int INF = (int)1e9;

//count: 연산 횟수
//now: 현재 만든 값
public void result(int count, int n, int now){
    //기저사례1 : n보다 now가 클 경우
    if(n < now) return;
    
    //중복계산 방지
    //기저사례2 : now의 값을 나누는 최소 몫의 경우에만 기록 후 계산 진행
    if(memo[now] > count) {
        memo[now] = count;
    }else {
        return;
    }
    
    //5를 선택하는 경우
    result(count+1, n, now+5);
    //3을 선택하는 경우
    result(count+1, n, now+3);

}
```