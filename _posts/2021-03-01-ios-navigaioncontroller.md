---
title:  "[iOS] custom navigation controller "
date:   2021-03-01
tags: [iOS]
toc: true 
categories: [iOS]
---

# Custom Navigation Controller

개발하다 보니 네비게이션쪽에 다음과 같은 요구사항이 생겼다.

1. 특정 뷰에서만 네비게이션바가 보여야한다.
2. 네비게이션 폰트, 색이 지정되어야 한다.
3. 왼쪽 back 버튼쪽을 원하는 이미지로 변경해야한다.
4. 네비게이션바 아래 border가 없어야 한다.



따라서 위의 요구사항을 고려하여 만들어봤다.

CommonNavigationController라는 클래스를 생성했으며, 네비게이션 바가 노출되어야하는 뷰컨트롤러에서 상속받게끔 구현했다.



```swift
class CommonNavigationController: UIViewController {
    
    override func viewDidLoad() {
        navigationController?.interactivePopGestureRecognizer?.delegate = nil
    }
    
    override func viewWillAppear(_ animated: Bool) {
        self.navigationController?.navigationBar.barTintColor = .white
        
        self.navigationController?.navigationBar.titleTextAttributes =
        [NSAttributedString.Key.foregroundColor: UIColor(rgb: 0x333333),
         NSAttributedString.Key.font: UIFont(name: "NotoSansKR-Medium", size: 16)!]
        
        self.navigationItem.leftBarButtonItem = UIBarButtonItem(
            image: UIImage(named: "14BackPickygray"),
            style: .plain,
            target: self,
            action: #selector(self.back(sender:))
        )
        
        self.navigationController?.navigationBar.clipsToBounds = true
        self.navigationController?.isNavigationBarHidden = false
    }
    
    override func viewWillDisappear(_ animated: Bool) {
        self.navigationController?.isNavigationBarHidden = true
    }

    @objc func back(sender: UIBarButtonItem) {
        _ = navigationController?.popViewController(animated: true)
    }
}
```

