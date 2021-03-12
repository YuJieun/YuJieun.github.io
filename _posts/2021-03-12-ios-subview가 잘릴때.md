---
title:  "[iOS] subView 잘림문제"
date:   2021-03-12
tags: [iOS]
toc: true 
categories: [iOS]
---

# 서브뷰(subView)가 잘릴때 해결하기

어떤뷰를 클릭하면 addsubView를 해줘야하는 경우가 있었다.

근데 add된 subView가 온전히 보여야하는데 부모뷰에 맞춰서 잘렸다.

해결방법은 다음과 같다.



## 해결방법

첫번째, 부모뷰의 clipToBounds를 off시켜준다.

그런데 이렇게 clipToBounds를 off시켜줘도 애가 자꾸 잘려서 여쭤보니까 셀일때 zposition문제가 있었다.

zposition을 조정함으로서 스크린의 레이어의 front-to-back순서를 조정할 수 있다.

따라서, 둘째로 부모셀보다 큰 subview를 add했을 때 zposition 값을 높여서 가장 layer가 위쪽으로 뜨게끔 수정하니

헤결이 되었다.





요 공통뷰를 개발하면서 UIView의 extension을 추가해주었는데,

"해당 뷰의 부모뷰를 탐색하면서, 내가 원하는 타입인 뷰가 있는지 체크하는 기능"이다.

```swift
extension UIView {
    public func checkViewFromParent<T>(_ type: T.Type) -> T? {
        var view: UIView? = self
        while view != nil {
            view = view?.superview
            if let view = view as? T {
                return view
            }
        }
        return nil
    }
}
```

나는 부모가 셀일때 zposition을 높여야 하는 코드가 있었으므로 type파라미터는 UICollectionViewCell.self 를 주어서 저 기능을 사용했다~ 끝!