---
title: '배열 돌리기 1'
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
<a href="https://www.acmicpc.net/problem/16926" target="_blank">https://www.acmicpc.net/problem/16926</a>

## 풀이
### 문제의 추상화
N*M 크기의 배열이 있을 때 R번 배열을 시계 반대 방향으로 회전시켜라

### 제한 사항
2 ≤ N, M ≤ 300
1 ≤ R ≤ 1,000
min(N, M) mod 2 = 0

### 문제의 계획
min(N,M) / 2 = 회전시켜야 하는 줄의 갯수
각 줄의 갯수마다 배열을 만든다
최대 갯수는 150줄
각 배열의 사이즈 % R만큼 왼쪽으로 이동

### 코드
```java
public int N, M, R;
public int[][] map;
public void result() {
    //n과 m 중 최소 길이가 2의 배수이기에 가능
    int count = Math.min(N, M) / 2;
    int[][] MOVE = {{1,0},{0,1},{-1,0},{0,-1}};
    //각 줄의 갯수마다 배열을 만든다
    ArrayList<ArrayList<Integer>> list = new ArrayList<ArrayList<Integer>>();
    //각 줄마다 배열 옮기기
    for(int i = 0; i < count; i++) {
        list.add(new ArrayList<Integer>());
        ArrayList<Integer> arr = list.get(i);
        int x,y,n,m;
        x = y = i;
        n = N-i;
        m = M-i;
        
        for(int mv = 0; mv < 4; mv++) {
            int length = n;
            if(mv == 1 || mv == 3) length = m;
            
            for(int j = i+1; j < length; j++) {
                arr.add(map[x][y]);
                x += MOVE[mv][0];
                y += MOVE[mv][1];
            }
        }
        
        x = y = i;
        int arrCount = 0;
        int move = R % arr.size();
        for(int mv = 0; mv < 4; mv++) {
            int length = n;
            if(mv == 1 || mv == 3) length = m;
            
            for(int j = 1+i; j < length; j++) {
                int nm = arrCount - move;
                if(nm < 0) nm = arr.size() + nm;
                
                map[x][y] = arr.get(nm);
                x += MOVE[mv][0];
                y += MOVE[mv][1];
                arrCount++;
            }
        }
    }
}
```