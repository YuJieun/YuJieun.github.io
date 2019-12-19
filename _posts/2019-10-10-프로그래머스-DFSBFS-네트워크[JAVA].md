---
title:  "프로그래머스 네트워크[DFS BFS/Java]"
date:   2019-10-10
tags: [algorithm]
toc: true 
---

문제 분류 : DFS/BFS



1. 내풀이

```java
import java.util.*;

class Solution {
    boolean[] visited;
    int answer=0;
    public int solution(int n, int[][] computers) {
        visited = new boolean[n];
        for(int i=0; i<n; i++){
            if(visited[i]==false){
                go(i,computers);
                answer++;
            }
        }        
        return answer;
    }
    
    public void go(int idx, int[][] computers){
        visited[idx]= true;
        for(int j=0; j<visited.length; j++){
            if(visited[j]==false && computers[idx][j]==1){
                go(j, computers);
            }
        }
        return;
    }
}

```

DFS로 풀었다





**오늘의 공부**

1. j = idx+1로 했다가 틀렸다. j 0부터 시작으로 바꾸니 해결
2. BFS로 풀려다가 헛짓함
3. 코드 다시보기