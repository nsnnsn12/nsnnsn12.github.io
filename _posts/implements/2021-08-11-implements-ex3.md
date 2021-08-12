---
title: '단어 뒤집기 2'
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
<a href="https://www.acmicpc.net/problem/17413" target="_blank">https://www.acmicpc.net/problem/17413</a>

## 풀이
### 문제의 추상화
1. 태그가 아닌 단어를 뒤집어라
2. 단어는 알파벳 소문자와 숫자로 이루어져 있고 공백으로 나누어져 있다.

### 제한 사항
1 ≤ 문자열의 길이 ≤ 100,000

### 문제의 계획
&lt;가 나오면 &gt;가 나올 때 까지 작동하지 않도록 flag 값을 넣는다.

### 코드
```java
public String result(String str) {
    String rel = "";
    int n = str.length();
    boolean isTag = false;
    Stack<Character> stack = new Stack<Character>();
    Queue<Character> queue = new LinkedList<Character>();
    for(int i = 0; i < n; i++) {
        char c = str.charAt(i);
        
        if(c == '<') {
            while(!stack.isEmpty()) {
                rel += stack.pop();
            }
            isTag = true;
            queue.add(c);
        }else if(c == '>') {
            isTag = false;
            queue.add(c);
            while(!queue.isEmpty()) {
                rel += queue.poll();
            }
        }else if(isTag) {
            queue.add(c);
        }else if(c == ' ') {
            while(!stack.isEmpty()) {
                rel += stack.pop();
            }
            rel += ' ';
        }else {
            stack.push(c);
        }
    }
    
    while(!stack.isEmpty()) {
        rel += stack.pop();
    }
    
    while(!queue.isEmpty()) {
        rel += queue.poll();
    }
    return rel;
}
```