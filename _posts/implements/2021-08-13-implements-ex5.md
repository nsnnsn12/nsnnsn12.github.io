---
title: '마인크래프트'
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
<a href="https://www.acmicpc.net/problem/18111" target="_blank">https://www.acmicpc.net/problem/18111</a>

## 풀이
### 문제의 추상화
1. N*M크기의 수열이 주어진다.
2. 수열의 값이 모두 동일하게 만들어라
3. 정수를 1씩 빼는 것은 2초, 정수를 1씩 더하는 것은 1초가 걸리다
4. 정수를 더하기 위해서는 기존의 값이 있거나 다른 정수에서 뺀 값을 사용할 수 있다.
5. 최소시간을 출력하라

### 제한 사항
첫째 줄에 N, M, B가 주어진다. (1 ≤ M, N ≤ 500, 0 ≤ B ≤ 6.4 × 107)  
0 <= 수열의 들어가는 정수 <= 256

### 문제의 계획
주어진 수열 내의 동일한 값끼리 배열을 이용하여 묶는다.  
ex)int[] list= new int[257];
수열 내의 최소값부터 최대값까지 범위 내 모든 값에  
대한 시간을 계산한다.

### 코드
```java
public final int INF = (int)1e9;
public final int HEIGHT = 257;
public int[] ground = new int[HEIGHT];
public int n;
public int m;
public int b;
public int min;
public int max;

public int[] standard(int min, int max, int B) {
    int rel= INF;
    int index = 0;
    int[] answer = {0, min};
    if(min == max) return answer;
    
    for(int i = min; i <= max; i++) {
        int second = 0;
        int b = B;
        for(int j = min; j <= max; j++) {
            if(i == j || ground[j] == 0) continue;
            int count = ground[j];
            if(i < j) {
                //기준 높이보다 높은 경우
                second += (j - i) * count * 2;
                b += (j - i) * count;
            }else {
                //기준 높이보다 낮은 경우
                second += (i - j) * count;
                b -= (i - j) * count;
            }
        }
        
        if(b < 0) continue;
        if(rel >= second) {
            rel = second;
            index = i;
        }
    }
    answer[0] = rel;
    answer[1] = index;
    return answer;
}
```