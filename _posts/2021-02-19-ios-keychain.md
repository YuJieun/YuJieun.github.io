---
title:  "[iOS] 키체인 Keychain"
date:   2021-02-19
tags: [iOS]
toc: true 
categories: [iOS]
---

# 키체인



## 개념정리

------

### 키체인이란?

키체인을 이용해 민감한 데이터를 안전하게 저장.
iOS에서 키체인은 사용자가 기기를 unlock하면 자동으로 unlock되고, lock하면 자동으로 lock된다.
각 앱은 오직 자신만의 키체인 아이템에 접근하거나, 그 앱이 속한 그룹에 있는데에만 접근할 수 있다.
키체인 서비스 API를 통해 키체인이라 불리는 암호화된 데이터베이스에 사용자의 데이터를 저장한다.
키체인은 샌드박스 외부에 위치하므로, 앱을 삭제에도 키체인에 저장된 정보는 삭제되지 않는다.

**샌드박스란?
-> ios시스템의 샌드박스는 앱마다 별도의 파일을 생성하여 공유되지 않도록 하고, 외부로부터의 접근을 보호해주므로 시스템이 부정적으로 조작되는 것을 막는 "보안 형태"를 의미한다.



### 키체인 아이템이란?

만약 패스워드같이 보안이 중요한 민감한 애를 저장하고 싶을때, 우리는 얘를 키체인 아이템으로 묶을 수 있다.
키체인 서비스는 키체인(디스크에 있는 데이터베이스)안에서 데이터의 암호화와 저장을 컨트롤한다.
이후에, 검증된 프로세스는 그 아이템을 찾고 복호화하는데 키체인 서비스를 사용하게 된다.

iOS에서는 모든 앱에서 사용할 수 있는 하나의 키체인만 제공하며, macOS에서는 애플리케이션이 원하는 만큼의 키체인을 생성할 수 있따.



### 키체인 아이템 클래스

kSecClass키와 함께 사용하는 value들

```
- kSecClassGenericPassword : 일반적 패스워드 아이템
- kSecClassInternetPassword : 인터넷용 패스워드 아이템
- kSecClassCertificate : 인증서 아이템
- kSecClassKey : 암호 키 아이템
- kSecClassIdentity : identity 아이템
```



### Attributes

키체인 아이템을 나타내는 특성. 아이템 클래스마다 다르다. 애플 공식 문서 참고!



## 키체인 구현

------

### 1. 키체인에 인터넷계정 add하기

```swift
//1. 저장할 structure를 생성해준다.
struct Credentials {
  var username: String
  var password: String
} 

//2. 키체인 접근 결과에 따른 에러enum을 정리해준다.
enum KeychainError: Error {
    case noPassword
    case unexpectedPasswordData
    case unhandledError(status: OSStatus)
}

//3. add query를 생성해준다.
let account = credentials.username
let password = credentials.password.data(using: String.Encoding.utf8)!
var query: [String: Any] = [kSecClass as String: kSecClassInternetPassword,
                            kSecAttrAccount as String: account,
                            kSecAttrServer as String: server,
                            kSecValueData as String: password]

//4. 아이템을 add한다.
let status = SecItemAdd(query as CFDictionary, nil)
guard status == errSecSuccess else { throw KeychainError.unhandledError(status: status) }

```



### 2. 일반 패스워드 add하기

```swift
//1. 패스워드 데이터
  guard let data = try? JSONEncoder().encode(user) else { return false }

//2. 쿼리
	let query: [CFString: Any] = [kSecClass: kSecClassGenericPassword,
                                  kSecAttrService: service,
                                  kSecAttrAccount: account,
                                  kSecAttrGeneric: data]

//3. 아이템 add
   return SecItemAdd(query as CFDictionary, nil) == errSecSuccess
```



### 3. 인터넷계정 조회하기

```swift
//1. 쿼리생성
let query: [String: Any] = [kSecClass as String: kSecClassInternetPassword,
                            kSecAttrServer as String: server,
                            kSecMatchLimit as String: kSecMatchLimitOne,
                            kSecReturnAttributes as String: true,
                            kSecReturnData as String: true]


//2. 접근
var item: CFTypeRef?
let status = SecItemCopyMatching(query as CFDictionary, &item)
guard status != errSecItemNotFound else { throw KeychainError.noPassword }
guard status == errSecSuccess else { throw KeychainError.unhandledError(status: status) }


//3. 결과 추출
guard let existingItem = item as? [String : Any],
    let passwordData = existingItem[kSecValueData as String] as? Data,
    let password = String(data: passwordData, encoding: String.Encoding.utf8),
    let account = existingItem[kSecAttrAccount as String] as? String
else {
    throw KeychainError.unexpectedPasswordData
}
let credentials = Credentials(username: account, password: password)
```



### 4. 일반 패스워드 조회

```swift
//1. 쿼리생성
let query: [CFString: Any] = [kSecClass: kSecClassGenericPassword,
                                kSecAttrService: service,
                                kSecAttrAccount: account,
                                kSecMatchLimit: kSecMatchLimitOne,
                                kSecReturnAttributes: true,
                                kSecReturnData: true]

//2. 접근 및 추출
var item: CFTypeRef?
if SecItemCopyMatching(query as CFDictionary, &item) != errSecSuccess { return nil }

guard let existingItem = item as? [CFString: Any],
let data = existingItem[kSecAttrGeneric] as? Data,
let user = try? JSONDecoder().decode(User.self, from: data) else { return nil }
```



### 5. 인터넷계정 삭제

```swift
//1. 쿼리 생성
let query: [String: Any] = [kSecClass as String: kSecClassInternetPassword,
                            kSecAttrServer as String: server]

//2. 삭제
let status = SecItemDelete(query as CFDictionary)
guard status == errSecSuccess || status == errSecItemNotFound else { throw KeychainError.unhandledError(status: status) }
```

