---
title:  "[iOS] UICollectionView 프로토콜"
date:   2021-02-19
tags: [iOS]
toc: true 
categories: [iOS]
---

# UICollectionView Protocol



## 1. UICollectionViewDataSource

(1) dataSource : 컬렉션뷰를 위한 데이터를 제공

(2) UICollectionViewDataSource : 컬렉션뷰를 위한 데이터를 가공하고 제공하기위한 함수들

```
 collectionView(_:numberOfItemsInSection:)
 collectionView(_:cellForItemAt:)
```



## 2. UICollectionViewDelegate

컬렉션뷰 안의 아이템과 상호작용하기 위한 함수들

```
collectionView(_:didDeselectItemAt:)
collectionView(_:didDeselectItemAt:)
요런애들...
```



## 3. UICollectionViewDelegateFlowLayout

grid(격자형태)의 레이아웃을 구성.

flow layout는 아이템, 헤더, 푸터들의 사이즈를 결정하기 위해 컬렉션뷰의 delegate와 함께 동작. 이 delegate는 바로 UICollectionViewDelegateFlowLayout. 

```
collectionView(_:layout:sizeForItemAt:)
collectionView(_:layout:minimumLineSpacingForSectionAt:)
이런애들...
```

