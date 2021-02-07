---
title:  "[iOS] Deployment target설정"
date:   2021-02-07
tags: [iOS]
toc: true 
categories: [iOS]
---



# iOS Deployment Target

최소 os지원 버전인 deployment target을 뭘로 설정할까 고민을 했다.

나는 NWPathMonitor요걸 사용하고 싶기 때문에(12부터 지원함). 12 이상이어야 했는데
그 이하 버전을 쓰는 사용자가 많을까봐 고민을 했다.



https://developer.apple.com/kr/support/app-store/

근데 여기서 보니까

81%가 14를 쓰고

17%이 13을 쓰고

나머지 2%가 나머지버전을 쓴다고 하니

deployment target을 12로 해도 문제없겠다 ^^