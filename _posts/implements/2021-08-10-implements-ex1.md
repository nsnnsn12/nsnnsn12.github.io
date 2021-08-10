---
title: '프린터 큐'
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
<a href="https://www.acmicpc.net/problem/1966" target="_blank">https://www.acmicpc.net/problem/1966</a>

## 풀이
### 문제의 추상화
1. 각 문서에는 중요도가 있다.
2. FIFO의 형태로 출력된다.
3. 단, 현재 문서보다 중요도가 높은 문서가 하나라도 있다면 이 문서를 queue의 가장 뒤에 배치한다.
4. 각 문서의 수와 중요도가 주어질 때 선택한 index의 문서가 몇 번째로 출력되는지 알아내라.

### 제한 사항
1 ≤ 테스트케이스 수 ≤ 100  
1 <= 문서의 갯수 <= 9  
1 <= 중요도 <= 9  

### 문제의 계획
문서의 순서관리는 큐를 이용해서 하고 중요도 관리는 계수 정렬을 이용한다.

### 코드
```java
public Queue que = new LinkedList<Integer>();

//list : 각 문서의 중요도 정보가 계수 정렬된 배열
//selectIndex : 선택한 문서 번호
public int result(int[] list, int selectIndex) {
    int rel = 0;
    que.clear();
    int n = list.length;
    
    //큐에 문서 순서 기록
    for(int i = 0; i < n; i++) {
        que.add(i);
    }
    
    while(!que.isEmpty()) {
        int index = (int)que.poll();
        int important = list[index];
        
        boolean isImportant = true;
        for(int i : list) {
            if(important < i) {
                isImportant = false;
                break;
            }
        }
        
        if(isImportant) {
            rel++;
            if(selectIndex == index) break;
            list[index] = 0;
        }else {
            que.add(index);
        }
    }
    return rel;
}
```