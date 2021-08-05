---
title: '화살표 그리기'
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
<a href="https://www.acmicpc.net/problem/15970" target="_blank">https://www.acmicpc.net/problem/15970</a>

## 풀이
### 문제의 추상화
1. n개의 위치가 있고 각 위치에 존재하는 점은 1~n까지의 색깔 종류 중 하나를 가진다.
2. 각 점을 연결한다. 단 같은 색깔의 점 중 가장 가까운 거리의 점을 연결한다.
3. 각 점의 길이를 더하여 합을 출력하라.

### 제한 사항
위치:x, 색깔 y  
0 ≤ x ≤ 100000, 1 ≤ y ≤ N

### 문제의 계획
색깔 n개의 대한 위치 정보 리스트를 만든다.
위치 정보에 해당하는 동일 색깔의 위치 정보와만 비교하여 최소 거리를 찾는다.

### 코드
```java
public ArrayList<ArrayList<Integer>> colorInfo = new ArrayList<ArrayList<Integer>>();
public int[][] positionList;
public final int INF = (int)1e9;

public int result() {
    int rel = 0;
    for(int[] positionInfo : positionList) {
        int minDistance = INF;
        int position = positionInfo[0];
        int color = positionInfo[1];
        for(int equalsColor : colorInfo.get(color)) {
            int distance = position - equalsColor;
            if(distance == 0) continue;
            if(distance < 0) distance *= -1;
            minDistance = Math.min(minDistance, distance);
        }
        rel += minDistance;
    }
    return rel;
}

public int reculsion(int depth, int n) {
    if(depth == n) return 0;
    
    int[] positionInfo = positionList[depth];
    
    int minDistance = INF;
    int position = positionInfo[0];
    int color = positionInfo[1];
    for(int equalsColor : colorInfo.get(color)) {
        int distance = position - equalsColor;
        if(distance == 0) continue;
        if(distance < 0) distance *= -1;
        minDistance = Math.min(minDistance, distance);
    }
    
    return minDistance + reculsion(depth+1, n);
}
```