---
title: '사탕 게임'
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
<a href="https://www.acmicpc.net/problem/3085" target="_blank">https://www.acmicpc.net/problem/3085</a>

## 풀이
### 문제의 추상화
1. n*n 크기의 맵이 주어질 때 색이 서로 다른 인접한 칸을 골라 값을 교환한다.
2. 교환 후 같은 색으로 이루어진 가장 긴 연속 부분의 길이를 출력하라.

### 제한 사항
3 <= n <= 50

### 문제의 계획
각 행과 아래 인접한 행끼리의 교환(최대 2500번)
각 열과 오른쪽에 인접한 열끼리의 교환(최대 2500번)
각 교환이 일어날 때 마다 교환한 행 혹은 열의 값을 검사(최대 5000 * 3 * 50 = 750000)

### 문제를 풀면서 생긴 애로사항
a[x][y] <-> a[x][y+1] 교환한 경우 각 행에 대한 연속 부분만 체크하고 열의 교환을 하지 않았고 그 반대의 경우도 마찬가지로 체크하지 못 했음.  
결국 처음에 문제가 쉽다고 생각하고 계획을 짤 때 대충 넘어가서 다 만들고서 계속 헤매느라 시간이 오래 걸림..

### 코드
```java
public char[][] map;
public int searchMaxCandyCount(int depth, boolean isWidth) {
    int n = map.length;
    
    if(depth == n) return 0;
    if(depth == n-1) return getEqualsCount(depth, isWidth);
    
    int count = getEqualsCount(depth, isWidth);
    
    for(int i = 0; i < n; i++) {
        if(isWidth) {
            switchPosition(depth, i, depth + 1, i);
            }
        else {
            switchPosition(i, depth, i, depth+1);
        }
        
        count = Math.max(count, getEqualsCount(depth, isWidth));
        count = Math.max(count, getEqualsCount(depth+1, isWidth));
        count = Math.max(count, getEqualsCount(i, !isWidth));
        
        if(isWidth) {
            switchPosition(depth, i, depth + 1, i);
            }
        else {
            switchPosition(i, depth, i, depth+1);
        }
    }
    
    count = Math.max(count, result(depth+1, isWidth));
    return count;
}

//행, 열의 대한 같은 갯수 조회
public int getEqualsCount(int flag, boolean isWidth) {
    int rel = 0;
    int n = map.length;
    
    int count = 1;
    if(isWidth) {
        for(int i = 1; i < n; i++) {
            if(map[flag][i-1] == map[flag][i]) {
                count++;
            }else {
                rel = Math.max(rel, count);
                count = 1;
            }
        }
    }else {
        for(int i = 1; i < n; i++) {
            if(map[i-1][flag] == map[i][flag]) {
                count++;
            }else {
                rel = Math.max(rel, count);
                count = 1;
            }
        }
    }
    rel = Math.max(rel, count);
    return rel;
}

//위치 교환
public void switchPosition(int x, int y, int nx, int ny) {
    char temp = map[x][y];
    map[x][y] = map[nx][ny];
    map[nx][ny] = temp; 
}
public int searchMaxCandyCount() {
    //행과 열에 대한 최대값 검색
    int rel = Math.max(result(0, true), result(0, false));
    
    return rel;
}
	
```