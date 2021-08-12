---
title: '숫자 정사각형'
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
<a href="https://www.acmicpc.net/problem/1051" target="_blank">https://www.acmicpc.net/problem/1051</a>

## 풀이
### 문제의 추상화
1. N*M크기의 직사각형이 있다
2. 각 칸은 숫자가 적혀 있다.
3. N*M크기의 직사각형 내의 각 꼭지점이 같은 숫자로 이루어진 최대 크기의 정사각형을 찾아 크기를 출력하라.

### 제한 사항
1 <= N,M <= 50

### 문제의 계획
모든 칸을 탐색한다고 했을 때 125,000(50*50*50)가지의 경우의 수가  
나오므로 완전탐색을 진행한다.

### 코드
```java
public int N;
public int M;
public int[][] map;
public int result() {
    int rel = 0;
    for(int x = 0; x < N; x++) {
        for(int y = 0; y < M; y++) {
            int flag = map[x][y];
            for(int m = y+1; m < M; m++) {
                if(flag == map[x][m]) {
                    int distance = m - y;
                    if(isSquare(flag, x, y, distance)){
                        distance += 1;
                        distance *= distance;
                        rel = Math.max(rel, distance);
                    }
                }
            }
        }
    }
    
    return rel;
}

public boolean isSquare(int flag, int x, int y, int distance) {
    boolean rel = true;
    int[][] move = {%raw%}{{distance, distance},{distance,0}}{%endraw%};
    for(int i = 0; i < 2; i++) {
        int nx = move[i][0] + x;
        int ny = move[i][1] + y;
        if(!isValueEquals(flag, nx, ny)) {
            return false;
        }
    }
    return rel;
}

public boolean isValueEquals(int flag, int nx, int ny) {
    boolean rel = true;
    if(nx < 0 || N <= nx ||ny < 0 || M <= ny) return false;
    if(map[nx][ny] != flag) return false;
    return rel;
}

public static void main(String[] args) {

    Scanner sc = new Scanner(System.in);
    N = sc.nextInt();
    M = sc.nextInt();
    map = new int[N][M];
    sc.nextLine();
    for(int i = 0; i < N; i++) {
        char[] str = sc.nextLine().toCharArray();
        for(int j = 0; j < M; j++) {
            map[i][j] = str[j] - '0';
        }
    }
    int rel = result();
    System.out.println(rel == 0? 1:rel);
}
```