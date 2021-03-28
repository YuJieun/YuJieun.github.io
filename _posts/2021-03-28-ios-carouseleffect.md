---
title:  "[iOS] collectionView carousel effect "
date:   2021-03-28
tags: [iOS]
toc: true 
categories: [iOS]
---

# collectionView carousel effect

collectionView를 가로스크롤할때 한장한장 딱 딱 페이지대로 넘기고 싶었다.

딱 오른쪽으로 넘기면 한장만 딱 넘겨지는 그 효과....

그걸 찾아보니까 carousel effect(회전목마 효과....?)라고 했다.

이걸 구현하려면 셀 사이즈와 scrollView.contentInset등을 조정해주어야하는데 이게 계산하는게 머리가 아팠다.

일단 완성 코드는 다음과 같다



```swift
func scrollViewWillEndDragging(_ scrollView: UIScrollView, withVelocity velocity: CGPoint, targetContentOffset: UnsafeMutablePointer<CGPoint>) {

        let layout = self.collectionView.collectionViewLayout as! UICollectionViewFlowLayout
        //320 : 셀사이즈
        let cellWidthIncludingSpacing = 320 + layout.minimumLineSpacing

        var offset = targetContentOffset.pointee
        let index = (offset.x + scrollView.contentInset.left) / cellWidthIncludingSpacing
        var roundedIndex = round(index)

        if scrollView.contentOffset.x > targetContentOffset.pointee.x { roundedIndex = floor(index) }
        else if scrollView.contentOffset.x < targetContentOffset.pointee.x {
            roundedIndex = ceil(index)
        } else {
            roundedIndex = round(index)
        }

        if currentIndex > roundedIndex {
            currentIndex -= 1
            roundedIndex = currentIndex
        } else if currentIndex < roundedIndex {
            currentIndex += 1
            roundedIndex = currentIndex
        }

        offset = CGPoint(x: (roundedIndex * cellWidthIncludingSpacing) - scrollView.contentInset.left, y: -scrollView.contentInset.top)
        targetContentOffset.pointee = offset

    }

```



1. 첫번째 해줘야할게 손가락으로 스크롤했을 때, 다음에 보여줘야하는애가 몇번째 셀인지 index를 구해줘야한다. 그래서 해당 index 셀 위치로 스크롤 이동해서 딱 보여줘야하기 떄문이다
2. 그러기 위해서 현재 스크롤한위치x값에 왼쪽 inset을 더한다음 셀사이즈로 나눠주었다. 그러면 몇번째 셀이 노출되어야하는지 index값이 나온다.

3. roundedIndex값이 도출되었으면, 그 값에 셀의 너비를 곱한 뒤 왼쪽 inset만큼 빼주면 보여줘야하는 셀의 x값의 위치가 나오게된다. ㅎㅎ



일단 나는 셀의 너비가 고정이었고, right inset값을 (화면너비 - 셀 너비 - 왼쪽 inset)과 동일하게 맞춰주어서 비교적 쉽게 계산할수있도록 했다.

근데 만약 셀 너비가 가변이고 오른쪽 inset이 다른값이라면 계산하기위해서 머리가 터져버릴 것이다.



4. pagingEnabled 값은 false로 해줘야한다!!(true로 했다가 자꾸 내가 원하는 위치에 스크롤이 안멈춰서 애먹음 ㅠㅠ)

![시연이미지](https://github.com/YuJieun/YuJieun.github.io/blob/master/assets/images/carousel.gif?raw=true)

완성된 화면은 위와 같다 끝!

