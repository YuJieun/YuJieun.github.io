---
title:  "[iOS] CollectionView에서 Cell을 사용할 때"
date:   2021-02-09
tags: [iOS]
toc: true 
categories: [iOS]
---



# CollectionView에서 Cell을 사용할때

CollectionView에서 Cell을 사용할때 공부한 것을 정리해본다.
기본적인 것이지만, 그동안 계속 헷갈렸기 때문에..ㅠ.ㅠ



## 1. CollectionView register

**재사용은 iOS 동작 최적화.시스템 자원을 위해 가장 중요한 부분**

collectionView에서 셀들은 dequeue되면서 재사용된다. (시스템에 의해 자동으로) 그러기 위해선 UICollectionViewCell을 register해주어야한다.

```swift
collectionView.register(UICollectionViewCell.self, forCellWithReuseIdentifier: "Cell")
```

재사용하려고 한 셀이 존재하지 않으면, 새로운 애가 자동으로 생성되고 이후 걔를 재사용. forCellReuseIdentifer은 재사용 식별자. nil이거나 empty string이면 안됨

------

Interface Builder nib file로 등록하고 싶으면,

```swift
collectionView.register(UINib(nibName: "yourNib", bundle: nil), forCellReuseIdentifier: "CellFromNib")
```

위의 방식이나 아래 방식 어떤 방식으로 등록하든, 셀을 dequeue하려면 아래처럼..

나는 xib가 있으니 아래 방식으로 register해줌..

* Xib : xml interface builder : 플랫파일
* Nib : next Interface builder : 바이너리 파일

xib를 nib로 바꾼 후 bundle에 올림. 빌드 시, xib -> nib





## 2. cell에서 init

* init(frame: CGRect) : Interface Builder 말고 programmatically하게  UIView상속받은 클래스 만들 때 사용. 
* required init?(coder aDecoder: NSCoder) : Interface Builder에서 생성되는 초기화.  
  내가 어떤 뷰를 만들고 있는데 거기에 미리 만둘어둔 셀을 xib상으로 추가하고 싶을때 사용했다ㅎ.ㅋ

객체가 init으로 초기화 (인스턴스화) 후 호출되는 애가 awakeFromNib. 





## 3. awakeFromNib()

awakeFromNib : nib파일 생성된 후 초기화 작업 준비. 셀은 viewDidLoad가 없음. 
얘를 거치고 UICollectionViewDataSource와  UICollectionViewDelegate탄다.

셀은 재사용되므로, 스크롤 내려서 셀이 재사용되면 걔네는 awakeFromNib호출이 안된다.