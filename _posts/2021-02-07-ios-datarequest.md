---
title:  "[iOS] Alamofire를 이용한 데이터통신"
date:   2021-02-07
tags: [iOS]
toc: true 
categories: [iOS]
---



## 서론

Alamofire를 이용해서 데이터통신 모듈을 만들어봤는데 만들면서 몇가지 공부한 것들이 생겨 정리해보았다.



## 1. 통신코드

(1) viewcontroller에서 데이터통신모듈 호출

```swift
DataRequest.getWeatherApI(){(isAutoLogin, data) in
            if isAutoLogin {
                appDelegate.switchHome()
            }
            else {
                appDelegate.switchLogin()
            }
        } failure: { errstr in
            print(errstr)
        }
```

(2) Networkmanager에 get통신하는 모듈 호출하여 데이터를 다시 viewcontroller에 보내는 중간단계

```swift
static func getWeatherApI(success: @escaping (Bool, DI_Weather) -> Void, failure: @escaping (_ str: String) -> Void ) {
        let url = ConstGroup.WEATHER_URL
        NetworkManager.getNetwork(url, DI_Weather.self, success: { (data) in
            success(false, data)
            
        }, failure: { errStr in
            failure(errStr)
        })
    }
```

(3) 실제 Alamofire를 이용해 통신하는 코드

```swift
class func getNetwork<T: Decodable>(_ url: String, _ type: T.Type, success: @escaping (T)-> Void, failure: @escaping (String) -> Void) {
        AF.request(url).responseJSON { response in
            switch response.result {
            case .success(let res):
                do {
                    let jsonData = try JSONSerialization.data(withJSONObject: res, options: .prettyPrinted)
                    let json = try JSONDecoder().decode(type, from: jsonData)
                    success(json)
                }
                catch(let err) {
                    failure("\(err.localizedDescription) _ catch부분")
                }
            case .failure(let err):
                failure("\(err.localizedDescription) _ failure부분")
            }
        }
    }
```





## 2. 왜 success와 failure에 @escaping을 붙여야 되나?

저 코드를 작성하면서, 파라미터로 success와 failure 클로져를 건네는데 자꾸 빨간불이 떴다.

**Escaping closure captures non-escaping parameter 'success'**

이게 왜 자꾸 뜨나 해서 escaping에 대해서 알아봤다. 내가 이해한바로는 이렇다.

1. 일단 escaping클로저는, 해당 함수의 인자로 클로저가 전달되지만, **함수가 반환된 후 실행**될때 사용한다. 함수의 인자가 함수의 영역을 탈출하여 **함수 밖**에서 사용할 수 있다는 개념이다.  
2. 내가 만든 getNetwork함수를 보면 AF.request를 호출하고, 비동기로 작동하여 response를 받아와 그제서야 completionHandler가 불린다. 

즉, 내가 이해한바로는 request요청을 하고 함수는 반환되며, 비동기로 response를 받아와 success클로저를 호출하므로, success클로저는 함수밖에서 사용되어야 하며

따라서 success는 escaping closure여야 한다는 것이다..!! (혹시 제가 틀렸다면 제발 말해주세여 ㅠㅠ)





## 3. <T: Decodable>?

일단 (3)의 getNetwork는 어떤 데이터클래스가 파라미터로 들어오더라도, 그 타입의 클래스로 decode되어야했다. 그러기 위해서 어떻게 어떻게 찾아서 <T: Decodable>를 사용함으로써 해결했는데 다시한번 정리를 해봐야겠다.

1. 일단 제네릭 타입이란, "타입을 파라미터화해서 컴파일시 구체적인 타입이 결정되도록 하는 것"이다. Any와 비슷하지만, 제네릭 타입을 가지는 대상에 타입이 정의되면 해당 제네릭은 그 타입만을 갖게된다. 따라서 재사용성 up

2. ```swift
   func test<T>(param: T){
     print(T)
   }
   
   test(param: "dd")
   test(param: 0)
   //이렇게 string과 int 다 가능가능
   ```

3. 제네릭 타입인 T는 Decodable 프로토콜이 없기 때문에, decode 하기 위해서 제네릭 T에 <T: Codable>로 프로토콜을 추가해주어야만 했다.



