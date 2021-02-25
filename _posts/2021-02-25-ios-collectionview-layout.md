---
title:  "[iOS] UICollectionView 레이아웃 정리"
date:   2021-02-25
tags: [iOS]
toc: true 
categories: [iOS]
---

# UICollectionView 레이아웃쪽 정리



1. contentSize : 모든 콘텐츠의 총 사이즈
2. contentInset : 상하좌우 여백을 주는 것. 바깥쪽 여백이 아닌 **안쪽** 여백을 주는 것.
3. contentOffset : x,y좌표를 의미하며 스크롤을 한다는 것 = contentOffset이 변하는 것. 
   즉, contentOffSet이 변하면 스크롤이 변하게 되는 것을 의미.
   contentOffset은 컨텐츠 영역이 아님

4. frame : 프레임의 원점은 superview의 원점으로부터 x,y축으로 얼마나 떨어져 있는가를 나타냄.

5. bound : 자기 자신의 좌표계 안에서 원점을 결정. 별도 값 할당하기 전까지는 항상 zero

   <img src="https://images.velog.io/images/cskim/post/b91dc941-0f47-4f8d-8734-0fe17aeaf514/frame-bounds-2.png" alt="frame과 bound" style="zoom:25%;" />

6. contentOffset.y 확인하기.

   <img src="https://www.programmersought.com/images/950/757caf352191f9f167c7f820f0b6aa86.png" alt="contentOffset." style="zoom:50%;" />

   