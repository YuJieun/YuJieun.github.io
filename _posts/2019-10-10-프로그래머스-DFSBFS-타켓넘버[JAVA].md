---
layout: post
title:  "프로그래머스 타겟넘버[DFS BFS/Java]"
date:   2019-10-10
categories: algorithm
---

문제 분류 : DFS/BFS



1. 내풀이

```java
import java.util.*;

class Solution {
    static int cnt=0;
    static int len, target_num;
    static int[] arr;
    public int solution(int[] numbers, int target) {
        len = numbers.length;
        target_num = target;
        arr = new int[len];
        for(int i=0; i<len; i++){
            arr[i] = numbers[i];
        }
        go(arr[0],0);
        go((-1)*arr[0],0);
        return cnt;
    }
    
    public void go(int num, int index){
        if(index+1 == len){
            if(num == target_num)
                cnt++;
            return;
        }else{
            go(num+arr[index+1],index+1);
            go(num-arr[index+1],index+1);
        }
    }
}
```

DFS로 풀었다







**오늘의 공부**

1. 이문제는 없음~