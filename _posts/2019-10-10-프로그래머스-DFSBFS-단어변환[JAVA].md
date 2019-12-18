---
layout: post
title:  "프로그래머스 단어변환[DFS BFS/Java]"
date:   2019-10-10
categories: algorithm
---

문제 분류 : DFS/BFS



1. 내풀이

```java
import java.util.*;

class Solution {
    boolean[] visited;
    int min = Integer.MAX_VALUE;
    int len;
    public int solution(String begin, String target, String[] words) {
        int answer = 0;
        visited = new boolean[words.length];
        len = begin.length();
        boolean flag = false;
        for(String tmp : words){
            if(tmp.equals(target))
                flag = true;
        }
        if(!flag)
            return 0;
        
        dfs(begin,0,words, target);
        return min;
    }
    
    public void dfs(String word, int cnt, String[] words, String target){
        if(word.equals(target)){
            if(cnt<min)
                min = cnt;
            return;
        }
        for(int i=0; i<words.length; i++){
            if(visited[i]==false){
                int check = 0;
                for(int j=0; j<len; j++){
                    if(words[i].charAt(j)!= word.charAt(j))
                        check++;
                }
                if(check==1){
                    visited[i] = true;
                    dfs(words[i],cnt+1,words,target);
                    visited[i] = false;
                }
            }
        }
    }
}
```

DFS로 풀었다

cnt++로 하면 안된다





2. 모범답안(이라기보단 BFS)

   ```java
   import java.util.LinkedList;
   import java.util.Queue;
   
   class Solution {
   
       static class Node {
           String next;
           int edge;
   
           public Node(String next, int edge) {
               this.next = next;
               this.edge = edge;
           }
       }
   
       public int solution(String begin, String target, String[] words) {
           int n = words.length, ans = 0;
   
           // for (int i=0; i<n; i++)
           //  if (words[i] != target && i == n-1) return 0;
   
           Queue<Node> q = new LinkedList<>();
   
   
           boolean[] visit = new boolean[n];
           q.add(new Node(begin, 0));
   
           while(!q.isEmpty()) {
               Node cur = q.poll();
               if (cur.next.equals(target)) {
                   ans = cur.edge;
                   break;
               }
   
               for (int i=0; i<n; i++) {
                   if (!visit[i] && isNext(cur.next, words[i])) {
                       visit[i] = true;
                       q.add(new Node(words[i], cur.edge + 1));
                   }
               }
           }
   
           return ans;
       }
   
       static boolean isNext(String cur, String n) {
           int cnt = 0;
           for (int i=0; i<n.length(); i++) {
               if (cur.charAt(i) != n.charAt(i)) {
                   if (++ cnt > 1) return false;
               }
           }
   
           return true;
       }    
   }
   ```

   



**오늘의 공부**

1. cnt++ 와 cnt+1의 차이..
2. DFS와 BFS로 둘다 푸는방법 알기