---
title: "자바스크립트 에러 모니터링"
date:  2020-03-17
tags: [Javascript]
toc: true
---

## 1. Javascript Errors and generic handling

### Error 객체

```javascript
throw new Error('something went wrong') 
```

위의 코드를 통해 에러 인스턴스 생성. 에러 처리를 하지 않으면, 실행 멈춘다.

Error object는 message + stack trace. 

```javascript
//message get하는 코드
const myError = new Error("something wrong")
console.log(myError.message)  //output:something wrong
```

Error의 stack property를 통해 stack trace 접근가능. error stack은 에러를 일으킨 파일에 대한 history전달.

```
Error: please improve your code
 at Object.<anonymous> (/Users/gisderdube/Documents/_projects/hacking.nosync/error-handling/src/general.js:1:79)
 at Module._compile (internal/modules/cjs/loader.js:689:30)
 at Object.Module._extensions...........
```



### Error다루기

```javascript
const a = 5
try {
    console.log(b) // b is not defined, so throws an error
} catch (err) {
    console.error(err) // will log the error with the error stack
} finally {
    console.log(a) // will always get executed
}
console.log(a) // still gets executed
```

만약 try,catch문이 없었으면 스크립트 실행이 멈춤



### 비동기 콜백 함수

비동기 함수가 있을 떄, 그 함수안에서 에러가 발생해도 스크립트 코드는 계속 실행되고 있을 것임.  그래서 에러가 즉각적으로 나오지 않을 것임. 콜백함수로 에러 핸들림 할 때 다음과 같이 2개의 parameters를 받을 것임.

```javascript
myAsyncFunc(someInput, (err, result) => {
    if(err) return console.error(err) // we will see later what to do with the error object.
    console.log(result)
})
```

에러가 발생하면 err parameter와 Error 동일해짐. 만약 그렇지 않으면 parameter는 undefined 또는 null일 것임. 



### 비동기 프로미스

```javascript
Promise.resolve(1)
    .then(res => {
        console.log(res) // 1

        throw new Error('something went wrong')

        return Promise.resolve(2)
    })
    .then(res => {
        console.log(res) // will not get executed
    })
    .catch(err => {
        console.error(err) // we will see what to do with it later
        return Promise.resolve(3)
    })
    .then(res => {
        console.log(res) // 3
    })
    .catch(err => {
        // in case in the previous block occurs another error
        console.error(err)
    })
```

catch블럭을 Promise에 넣어주기~