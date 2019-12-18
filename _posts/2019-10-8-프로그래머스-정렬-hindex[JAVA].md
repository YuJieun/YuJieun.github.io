---
layout: post
title:  "프로그래머스 H-Index[정렬/Java]"
date:   2019-10-8
category: algorithm
---

문제 분류 : 정렬



1. 내풀이

```java
import java.util.*;

class Solution {
    public int solution(int[] citations) {
        int answer = 0;

        Arrays.sort(citations);
        
        for(int i = 0; i < citations.length; i++) {
            if (citations[i]>= citations.length-i) {
                answer = citations.length-i;
                break;
            }
        }  
        return answer;   
    }
}
```

이거 코드는 짧은데 방법 생각하는게 어려움 ㅠㅠ


**오늘의 공부**

1. 코드 한번 다시 보기