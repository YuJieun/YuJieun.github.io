---
title: "자바스크립트의 비동기"
date:  2020-03-17
tags: [Javascript]
toc: true
---

https://joshua1988.github.io/web-development/javascript/promise-for-beginners/ 를 보면서 공부함. 

## 1. 비동기 처리

자바스크립트의 비동기 처리는 특정 코드의 연산이 끝날때까지 기다려주지 않고 다음 코드를 실행하는 특성을 말함.

## 2. 비동기 처리 예시

비동기 처리의 가장 흔한 사례는 제이쿼리의 ajax통신. 

```javascript
function getData() {
	var tableData;
	$.get('https://domain.com/products/1', function(response) {
		tableData = response;
	});
	return tableData; //기다려주지 않고 얘를 먼저 수행해버림
}

console.log(getData()); // undefined
```

console.log를 찍어봤을 때 받아온 데이터가 뭐든 찍혀야 하는데 결과는 **undefined**로 나온다. 왜일까? ajax로 데이터를 요청하고 받아올 때까지 기다려주지 않고 다음 코드인 return tableData를 수행해버림.

이렇게 특정 로직의 실행이 끝날 때까지 기다려주지 않고 나머지 코드를 실행해버리는 것이 비동기 처리. 

그러면 자바스크립트에서 왜 비동기 처리가 필요한가? 화면에서 서버로 데이터를 요청했을 때 서버가 언제 요청에 대한 답을 줄지도 모르는데 마냥 기다리기만 할 수는 없기 때문. 요청이 많을수록 그만큼 더 많이 기다리게 됨.

다음 예시로는,

```javascript
// #1
console.log('Hello');
// #2
setTimeout(function() {
	console.log('Bye');
}, 3000);
// #3
console.log('Hello Again');
```

setTimeout()은 코드를 지정한 시간만큼 기다렸다가 실행시킴.

기대했던 것 : Hello -> 3초 뒤 Bye출력 -> Hello Again출력

실제 나오는 것 : Hello -> Hello Again -> 3초 뒤 Bye

이것도 마찬가지로 3초를 기다렸다가 다음 코드를 수행하는 것이 아니라 일단 setTimeout()자체를 실행하고 나서 바로 다음 코드인 console.log('Hello Again')으로 넘어감.  결국, 하나하나 아래로 쭉 실행되고, 그 하나가 실행되는 것은 알아서~(기다려주지 않음)

## 3. 콜백 함수 - 비동기 처리 방식 문제점 해결

위의 ajax통신 코드를 콜백 함수로 개선해보자

```javascript
function getData(callbackFunc) {
	$.get('https://domain.com/products/1', function(response) {
		callbackFunc(response); // 서버에서 받은 데이터 response를 callbackFunc() 함수에 넘겨줌
	});
}

getData(function(tableData) {
	console.log(tableData); // $.get()의 response 값이 tableData에 전달됨
});
```

그런데

```javascript
$.get('url', function(response) {
	parseValue(response, function(id) {
		auth(id, function(result) {
			display(result, function(text) {
				console.log(text);
			});
		});
	});
});
```

얘처럼 콜백지옥이 발생할수도 있음. 화면에 표시하기까지 인코딩, 사용자 인증 등등을 처리해야 하는 경우... 콜백 안에 콜백을 계속 무는 형식으로 코딩을 하게 되는데 이런 코드는 가독성이 떨어지고 로직이 복잡해짐 --> **콜백 지옥**

## 4. 콜백 지옥 해결

Promise나 Async를 사용하면 콜백 지옥 해결 가능.

그런데 만약에 코딩 패턴으로만 콜백 지옥을 해결하려면 콜백 함수를 분리해주면 됨.

```javascript
function parseValueDone(id) {
	auth(id, authDone);
}
function authDone(result) {
	display(result, displayDone);
}
function displayDone(text) {
	console.log(text);
}
$.get('url', function(response) {
	parseValue(response, parseValueDone);
});
```

ajax 통신으로 받은 데이터를 parseValue() 메서드로 파싱. 그 다음에 차례로 메서드가 수행됨. 마지막에 text가 콘솔에 출력.

## 5. Promise

promise는 비동기 처리에 사용되는 객체. 주로 서버에서 받아온 데이터를 화면에 표시할 때 사용. 

```javascript
function getData(callbackFunc) {
  $.get('url 주소/products/1', function(response) {
    callbackFunc(response); // 서버에서 받은 데이터 response를 callbackFunc() 함수에 넘겨줌
  });
}

getData(function(tableData) {
  console.log(tableData); // $.get()의 response 값이 tableData에 전달됨
});
```

얘를 promise를 적용하면 아래와 같이 됨.

```javascript
function getData(callback) {
  // new Promise() 추가
  return new Promise(function(resolve, reject) {
    $.get('url 주소/products/1', function(response) {
      // 데이터를 받으면 resolve() 호출
      resolve(response);
    });
  });
}

// getData()의 실행이 끝나면 호출되는 then()
getData().then(function(tableData) {
  // resolve()의 결과 값이 여기로 전달됨
  console.log(tableData); // $.get()의 reponse 값이 tableData에 전달됨
});
```

프로미스에는 3가지 상태가 있는데,

1. Pending(대기) : 비동기 처리 로직이 아직 완료되지 않음
2. Fulfilled(이행) : 비동기 처리가 완료되어 프로미스가 결과 값을 반환해줌
3. Rejected(실패) : 비동기 처리가 실패하거나 오류 발생

------

### Pending

new Promise(); 메서드를 호출하면 대기 상태가 됨. 

new Promise를 호출할 때 콜백 함수 선언 가능. 인자는 resolve, reject임.

```javascript
new Promise(function(resolve, reject) {
  // ...
});
```

### Fulfilled

콜백 함수의 인자 resolve를 실행하면 이행상태가 됨. resolve();

이행 상태가 되면 then()을 이용하여 처리 결과 값 받을 수 있음.

```javascript
function getData() {
  return new Promise(function(resolve, reject) {
    var data = 100;
    resolve(data);
  });
}

// resolve()의 결과 값 data를 resolvedData로 받음
getData().then(function(resolvedData) {
  console.log(resolvedData); // 100
});
```

### Rejected

reject를 호출하면 실패 상태가 됨. reject();

실패상태이면 실패한 이유를 catch()로 받을 수 있음.

```javascript
function getData() {
  return new Promise(function(resolve, reject) {
    reject(new Error("Request is failed"));
  });
}

getData().then().catch(function(err) {
  console.log(err); // Error: Request is failed
});
```

### 예제

```javascript
function getData() {
  return new Promise(function(resolve, reject) {
    $.get('url 주소/products/1', function(response) {
      if (response) {
        resolve(response);
      }
      reject(new Error("Request is failed"));
    });
  });
}

getData().then(function(data) {
  console.log(data); 
}).catch(function(err) {
  console.error(err); 
});
```

## 

## 6. Promise Chaining

프로미스는 여러 개를 연결하여 사용가능. 

```java
new Promise(function(resolve, reject){
  setTimeout(function() {
    resolve(1);
  }, 2000);
})
.then(function(result) {
  console.log(result); // 1
  return result + 10;
})
.then(function(result) {
  console.log(result); // 11
  return result + 20;
})
.then(function(result) {
  console.log(result); // 31
});
```



## 7. Promise Error Handling

위의 예제들은 정상동작한다고 가정한 예제이고, 실제로는 오류가 발생할 수 있으므로 처리 방법도 알아야함. then()의 두번째 인자로 에러를 처리하거나 or catch()를 이용함

```javascript
function getData() {
  return new Promise(function(resolve, reject) {
    reject('failed');
  });
}

// 1. then()의 두 번째 인자로 에러를 처리하는 코드
getData().then(function() {
  // ...
}, function(err) {
  console.log(err);
});

// 2. catch()로 에러를 처리하는 코드
getData().then().catch(function(err) {
  console.log(err);
});
```

근데 가급적이면 **catch()로 처리하는게 더 효율적.**

## 8. async & await

자바스크립트 비동기 처리 패턴 중 가장 최근에 나온 문법. 기존의 콜백 함수와 프로미스의 단점 보완하고 개발자가 읽기 좋은 코드를 작성할 수 있게 해줌. (개발자는 위에서 아래로 한 줄 한 줄 읽는게 편함. )

```javascript
async function 함수명() {
  await 비동기_처리_메서드_명();
}
```

함수의 앞에 async 예약어 붙이고, 함수의 내부 로직 중 HTTP 통신을 하는 비동기 처리 코드 앞에 await을 붙임.

```javascript
async function logName() {
  var user = await fetchUser('domain.com/users/1');
  if (user.id === 1) {
    console.log(user.name);
  }
}
```

**주의해야할 점은 비동기 처리 메서드가 꼭 프로미스 객체를 반환해야 await가 의도한 대로 동작함!**

## 9. async & await 간단예제

```javascript
function fetchItems() {
  return new Promise(function(resolve, reject) {
    var items = [1,2,3];
    resolve(items)
  });
}

async function logItems() {
  var resultItems = await fetchItems();
  console.log(resultItems); // [1,2,3]
}
```



## 10. async & await 예외 처리

```javascript
async function logTodoTitle() {
  try {
    var user = await fetchUser();
    if (user.id === 1) {
      var todo = await fetchTodo();
      console.log(todo.title); // delectus aut autem
    }
  } catch (error) {
    console.log(error);
  }
}
```

catch{} 사용~!