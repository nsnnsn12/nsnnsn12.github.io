---
title: '오목'
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
<a href="https://www.acmicpc.net/problem/2615" target="_blank">https://www.acmicpc.net/problem/2615</a>

## 풀이
### 문제의 추상화
문제의 추상화   
19 * 19 크기의 map이 주어지고 오목을 둔다고 했을 때
검은 바둑은 1, 흰 바둑은 2, 바둑이 놓아지지 않은 곳은 0으로 친다.
검은색이 이기면 1, 흰색이 이기면 2, 무승부는 0을 출력하고 
이겼을 경우에는 가장 왼쪽에 있는 바둑알 세로일 경우는 상단의 좌표를 출력하라

### 제한 사항
1 <= N <= 100,000

### 문제의 계획
각 칸마다 오목이 가능한지 확인  
각 칸마다 최악의 경우의 수 8 * 6 = 48  
모든 칸 19* 19 = 361  
모든 경우의 수 361 * 48 = 17,328

### 코드
```java
public final int N = 19;
public int[][] map = new int[N][N];
public int[][] move = {%raw%}{{-1,-1},{1,1},{-1,0},{1,0},{1,-1},{-1,1},{0,-1},{0,1}}{%endraw%};
public int[] result() {
    int[] rel = new int[3];
    rel[0] = 0;
    rel[1] = 0;
    rel[2] = 0;
    for(int i = 0; i < N; i++) {
        for(int j = 0; j < N; j++) {
            if(map[j][i] != 0) {
                for(int m = 0; m < 8; m+=2) {
                    int[] count1 = check(j, i, m, map[j][i], 0);
                    int[] count2 = check(j, i, m+1, map[j][i], 0);
                    
                    if(count1[2] + count2[2]-1 == 5) {
                        rel[0] = map[j][i];
                        rel[1] = j+1;
                        rel[2] = i+1;
                        return rel;
                    }
                }
            }
        }
    }
    return rel;
}

public int[] check(int x, int y, int m, int c, int count) {
    int[] rel = new int[3];
    if(x >= N || x < 0 || y >= N || y < 0) {
        rel[0] = x;
        rel[1] = y;
        rel[2] = count;
        return rel;
    }
    if(count == 6) {
        rel[0] = x;
        rel[1] = y;
        rel[2] = count;
        return rel;
    }
    
    if(map[x][y] == c) {
        int nx = x + move[m][0];
        int ny = y + move[m][1];
        return check(nx, ny, m, c, count+1);
    }
    rel[0] = x;
    rel[1] = y;
    rel[2] = count;
    return rel;
}
```