---
title: '두 스티커'
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
<a href="https://www.acmicpc.net/problem/16937" target="_blank">https://www.acmicpc.net/problem/16937</a>

## 풀이
### 문제의 추상화
1. h*w 크기인 사각형 안의 두 개의 사각형을 넣는다 했을 때 최댓값을 출력하라.
2. 붙이는 사각형은 n개가 주어진다.
3. 붙일 때 90도 회전이 가능하며 맵을 벗어나거나 두개의 사각형이 겹쳐서는 안 된다.

### 제한 사항
1 ≤ H, W, N ≤ 100  
1 ≤ Ri, Ci ≤ 100

### 문제의 계획
n이 백이라 했을 때 2개로 조합할 수 있는 경우의 수는 4950
오른쪽 혹은 아래로 붙이는 경우
90도로 회전하는 경우 회전하지 않는 경우(스티커 두개의 다 해당)
4950 * 2 * 4 = 39600

### 코드
```java
public int[][] stickerList;
public int h;
public int w;
public int n;
public int result(int depth) {
    if(depth == n) return 0;
    
    int rel = 0;
    int fh = stickerList[depth][0];
    int fw = stickerList[depth][1];
    int ftotal = fh * fw;
    
    for(int i = depth+1; i < n; i++) {
        int rh = stickerList[i][0];
        int rw = stickerList[i][1];
        int total = (rh * rw) + ftotal;
        
        if(isBuilt(fh, fw, rh, rw)) {
            rel = Math.max(rel, total);
            continue;
        }
        
        //기준 값을 90도로 회전하는 경우
        if(isBuilt(fw, fh, rh, rw)) {
            rel = Math.max(rel, total);
            continue;
        }
        
        //둘다 회전하는 경우
        if(isBuilt(fw, fh, rw, rh)) {
            rel = Math.max(rel, total);
            continue;
        }
        //상대 값만 90도로 회전하는 경우
        if(isBuilt(fh, fw, rw, rh)) {
            rel = Math.max(rel, total);
            continue;
        }
    }
    
    return Math.max(rel, result(depth + 1));
}

public boolean isBuilt(int fh, int fw, int rh, int rw) {
    int totalH = 0;
    int totalW = 0;
    //오른쪽으로 붙였을 경우
    totalH = Math.max(fh, rh);
    totalW = fw + rw;
    if(totalH <= h && totalW <= w) return true;
    
    //아래로 붙였을 경우
    totalH = fh+rh;
    totalW = Math.max(fw, rw);
    if(totalH <= h && totalW <= w) return true;
    
    return false;
}
```