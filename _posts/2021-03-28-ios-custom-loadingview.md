---
title:  "[iOS] custom loadingView"
date:   2021-03-28
tags: [iOS]
toc: true 
categories: [iOS]
---

# custom LoadingView만들기

화면이동시, 로딩중이거나 웹뷰를 띄우거나 등등 로딩할때 loadingView를 띄우고자 했는데

내가 원하는 spinner이미지로 띄우고 싶었다.



### 로딩 스피너 이미지 찾기

먼저, https://loading.io/ 요기서 내가 원하는 스피너 gif파일을 다운로드 받았다.

이런 사이트가 있었다니 완전 최고! ㅋㅋㅋㅋ



### 로딩할때 띄우는 view생성하기

[요기](http://minsone.github.io/mac/ios/easy-make-loading-animation-popup-view-in-swift)를 참고해서 구현을 했다

```swift
//
//  CustomLoadingView.swift
//
//  Created by 유지은 on 2021/03/20.
//

import Foundation
import UIKit
import Gifu

class CustomLoadingView {
    private static let sharedInstance = CustomLoadingView()
        
    private var backgroundView: UIView?
    private var popupView: GIFImageView?
        
    class func show() {
      //view를 show해주는 코드
    }
    
    class func hide() {
      //view를 hide해주는 코드
    }
}

```

전체적인 구조는 위와 같다. 뷰는 싱글톤이며, 객체는 private으로 지정해주었다.

backgroundview는 로딩스피너 뒤에 흐린불투명한 뷰를 깔아주기 위해 생성했으며, popupviews는 스피너 gif를 위한 뷰이다.





### gif띄우기

```swift
let popupView = GIFImageView()
popupView.contentMode = .center
popupView.prepareForAnimation(withGIFNamed: "spinner")
```

[GIFU라이브러리](https://github.com/kaishin/Gifu)를 사용해서 gif를 보여주었다. (세상엔 개발이 급할때 쓰기 정말 좋은 라이브러리가 많다~) 사용하기 정말 간편~





### 로딩뷰 show

최상위뷰에 뷰를 붙이고 싶었다. 구글을 찾아보니 예제에는 UIApplication.shared.keyWindow 요거를 써서 addsubview를 해주던데, 요게 ios13에서 deprecated되었기 때문에 **if** **#available**(**iOS** 13.0, *) 분기처리 해주었다.

```swift
if #available(iOS 13.0, *) {
            if let window = UIApplication.shared.connectedScenes
                .filter({$0.activationState == .foregroundActive})
                .map({$0 as? UIWindowScene})
                .compactMap({$0})
                .first?.windows
                .filter({$0.isKeyWindow}).first {
                window.addSubview(backgroundView)
                window.addSubview(popupView)
                
                backgroundView.frame = CGRect(x: 0, y: 0, width: window.frame.maxX, height: window.frame.maxY)
                backgroundView.backgroundColor = UIColor(red: 0, green: 0, blue: 0, alpha: 0.2)
            
                popupView.frame = CGRect(x: 0, y: 0,width: window.frame.maxX, height: window.frame.maxY)
                popupView.startAnimatingGIF()
                
                sharedInstance.backgroundView?.removeFromSuperview()
                sharedInstance.popupView?.removeFromSuperview()
                sharedInstance.backgroundView = backgroundView
                sharedInstance.popupView = popupView
            }
        } else {
            if let window = UIApplication.shared.keyWindow {
                window.addSubview(backgroundView)
                window.addSubview(popupView)
                
                backgroundView.frame = CGRect(x: 0, y: 0, width: window.frame.maxX, height: window.frame.maxY)
                backgroundView.backgroundColor = UIColor(red: 0, green: 0, blue: 0, alpha: 0.2)
                
                popupView.frame = CGRect(x: 0, y: 0,width: window.frame.maxX, height: window.frame.maxY)
                popupView.startAnimatingGIF()

                sharedInstance.backgroundView?.removeFromSuperview()
                sharedInstance.popupView?.removeFromSuperview()
                sharedInstance.backgroundView = backgroundView
                sharedInstance.popupView = popupView
            }
}
```





### 로딩뷰 hide

```swift
class func hide() {
        if let popupView = sharedInstance.popupView,
        let backgroundView = sharedInstance.backgroundView {
            popupView.stopAnimatingGIF()
            backgroundView.removeFromSuperview()
            popupView.removeFromSuperview()
        }
    }
```

로딩뷰를 숨기고 싶을때 사용하는 hide함수는 위와 같이 구현했다 





### 사용하기

```swift
CustomLoadingView.show()
/*
통신이나
loading중~~~

*/
CustomLoadingView.hide()
```

show와 hide함수는 가까이 위치해주는게 이후 관리하거나 코드 흐름 파악하기 좋다~!





![로딩뷰이미지](https://github.com/YuJieun/YuJieun.github.io/blob/master/assets/images/loadingview.gif?raw=true)

완성화면은 위와 같다.(혹시 몰라서 위아래를 살짝 잘랐다. 스피너 돌아가는부분만 보면 된다)





근데 블로그쓰는거 정말 귀찮은데 다른 사람들은 어떻게 그렇게 성실하게 쓰는걸까

끝!



