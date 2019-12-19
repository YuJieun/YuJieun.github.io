---
title:  "프로그래머스 여행경로[DFS BFS/Java]"
date:   2019-10-10
tags: [algorithm]
toc: true 
---

문제 분류 : DFS/BFS



1. 내풀이이자 모범답안.... (힌트 얻으면서 함)

```java
import java.util.*;

class Solution {
    boolean[] visited;
    String tmp="";
    ArrayList<String> arr = new ArrayList<>();
    
    public String[] solution(String[][] tickets) {
        String[] answer ={};
        for(int i=0; i<tickets.length; i++){
            if(tickets[i][0].equals("ICN")){
                visited = new boolean[tickets.length];
                tmp = "ICN";
                visited[i] = true;
                dfs(tickets[i][1],tickets, 1);
            }
        }
        Collections.sort(arr);
        answer = arr.get(0).split(",");
        return answer;
    }
    
    public void dfs(String airport, String[][] tickets, int cnt){
        tmp = tmp +","+airport+"";
        if(cnt==tickets.length){
            arr.add(tmp);
            return;
        }
        for(int j=0; j<tickets.length; j++){
            if(visited[j]==false && airport.equals(tickets[j][0])){
                visited[j] = true;
                dfs(tickets[j][1],tickets,cnt+1);
                visited[j] = false;
                tmp = tmp.substring(0, tmp.length()-4);
            }
        }
    }
}

```



**오늘의 공부**

1. 문자열 비교는 반드시 equals (==을 썼다가 답이 안나옴)

2. 우선순위 큐 사용방법

3. class Object implements Comparable<Object>

   (위에까지는 다른 방법으로 풀면서 공부한 것들)

4. 코드 다시한번 살펴보기
5. visited배열이 선언되는 위치
6. tmp.length()-4

