# REACT API 연동하기

## API 연동의 기본
### REST API

1. 통신 규칙 (웹 통신 규약 http를 사용)

    print, document.write 같은 하나하나가 api라고 할 수 있다.

```jsx
       print('hello world')
    
       document.write('hello world')
```

2. rest api도 컴퓨터의 기능을 실행시키는 명령어

    특정 URI를 통해 데이터를 가져와서 출력한다.
```jsx
        ex) google calendars
       https://www,googleapis.com/.../calendars/calendars_id
        {
            "summary" : "일정",
            "timeZone" : "Asia/Seoul"
        }
```

이런식으로 http를 사용하여 기계들끼리 통신하기 위한 방법으로 볼 수 있다.

   [데이터 구조]
   
   ![1.png](1.png)

3. method

   이런 데이터를 가공을 하기 위해 아래 그림과 같은 4가지의 작업을 하게 되는데 이런 작업들을 method라 부른다


   [데이터 가공]

   ![2.png](2.png)
   
[rest API 실 습 ](http://localhost:8080)

[REACT 실 습](http://localhost:3000)

### Promise

프로미스는 자바스크립트 비동기 처리에 사용되는 객체입니다. 여기서 자바스크립트의 비동기 처리란 ‘특정 코드의 실행이 완료될 때까지 기다리지 않고 다음 코드를 먼저 수행하는 자바스크립트의 특성’을 의미한다.

1. Promise가 필요한 이유

   API가 실행되면 서버에다가 ‘데이터 하나 보내주세요’ 라는 요청을 보내게 되는데 여기서 데이터를 받아오기도 전에 마치 데이터를 다 받아온 것 마냥 화면에 데이터를 표시하려고 하면 오류가 발생하거나 빈 화면이 뜹니다. 이와 같은 문제점을 해결하기 위한 방법 중 하나가 프로미스입니다


```jsx
   [jQuery ajax]

   function getData(callbackFunc) {
    $.get('url 주소/products/1', function(response) {
        callbackFunc(response); // 서버에서 받은 데이터 response를 callbackFunc() 함수에 넘겨줌
    });
   }
   
   getData(function(tableData) {
    console.log(tableData); // $.get()의 response 값이 tableData에 전달됨
   });
```
제이쿼리의 ajax 통신 API를 이용하여 지정된 url에서 1번 상품 데이터를 받아오는 코드입니다. 비동기 처리를 위해 프로미스 대신에 콜백 함수를 사용


```jsx
   [Promise 적용]

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
콜백 함수로 처리하던 구조에서 new Promise(), resolve(), then()와 같은 프로미스 API를 사용한 구조

2. Promise 3가지 상태

- Pending(대기) : 비동기 처리 로직이 아직 완료되지 않은 상태
- Fulfilled(이행) : 비동기 처리가 완료되어 프로미스가 결과 값을 반환해준 상태
- Rejected(실패) : 비동기 처리가 실패하거나 오류가 발생한 상태

** Pending(대기)

```jsx
   new Promise();
    // new Promise() 메서드를 호출하면 대기(Pending) 상태

   new Promise(function(resolve, reject) {
      // ... 
   });
    //new Promise() 메서드를 호출할 때 콜백 함수를 선언할 수 있고, 콜백 함수의 인자는 resolve, reject입니다.
```

** Fulfilled(이행)
```jsx
   new Promise(function(resolve, reject) {
    resolve();
   });
    // 콜백 함수의 인자 resolve를 실행하면 이행(Fulfilled) 상태

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
    //이행 상태가 되면 then()을 이용하여 처리 결과 값을 받을 수 있습니다
```

** Rejected(실패)
```jsx
   new Promise(function(resolve, reject) {
      reject();
   });
    //  reject를 호출하면 실패(Rejected) 상태

   function getData() {
      return new Promise(function(resolve, reject) {
         reject(new Error("Request is failed"));
      });
   }
   
   // reject()의 결과 값 Error를 err에 받음
   getData().then().catch(function(err) {
      console.log(err); // Error: Request is failed
   });
    //실패 상태가 되면 실패한 이유(실패 처리의 결과 값)를 catch()로 받을 수 있습니다.
```

3. Promise 예제

```jsx
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
   
   // 위 $.get() 호출 결과에 따라 'response' 또는 'Error' 출력
   getData().then(function(data) {
      console.log(data); // response 값 출력
   }).catch(function(err) {
      console.error(err); // Error 출력
   });
```
서버에서 제대로 응답을 받아오면 resolve() 메서드를 호출하고, 응답이 없으면 reject() 메서드를 호출, 호출된 메서드에 따라 then()이나 catch()로 분기하여 응답 결과 또는 오류를 출력합니다.

### async & await
async와 await는 자바스크립트의 비동기 처리 패턴 중 가장 최근에 나온 문법입니다. 
기존의 비동기 처리 방식인 콜백 함수와 프로미스의 단점을 보완하고 개발자가 읽기 좋은 코드를 작성할 수 있게 도와줍니다.

```jsx
//user라는 변수에 객체를 할당한 뒤 조건문으로 사용자의 아이디를 확인하고 콘솔에 사용자의 name 출력해라
   var user = {
     id: 1,
     name: 'Josh'
   };
   if (user.id === 1) {
     console.log(user.name); // Josh
   }
```
위의 조건을 async & await를 간단히 적용하게 되면 아래와 같습니다.

```jsx
async function logName() {
    //fetchUser() 메서드가 서버에서 사용자 정보를 가져오는 HTTP 통신 코드라고 가정
   var user = await fetchUser('domain.com/users/1');
   if (user.id === 1) {
      console.log(user.name);
   }
}
```

예전빙식은 아래와 같습니다.
```jsx
function logName() {
   var user = fetchUser('domain.com/users/1', function(user) {
      if (user.id === 1) {
         console.log(user.name);
      }
   });
}
```
fetchUser()라고 하는 코드는 서버에서 데이터를 받아오는 HTTP 통신 코드라고 가정하고,
일반적으로 자바스크립트의 비동기 처리 코드는 아래와 같이 콜백을 사용해야지 코드의 실행 순서를 보장받을 수 있기때문에 이렇게 사용했습니다.
콜백을 사용하지 않는다면, 콘솔에는 undefinde가 나타날것 입니다.


#### async & await 간단한 예제

```jsx
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
- fetchItems() 함수는 프로미스 객체를 반환하는 함수(프로미스는 “자바스크립트 비동기 처리를 위한 객체“)
- fetchItems() 함수를 실행하면 프로미스가 이행(Resolved)되며 결과 값은 items 배열이 됨
- logItems() 함수를 실행하면 fetchItems() 함수의 결과 값인 items 배열이 resultItems 변수에 담김
- 콘솔에는 [1,2,3]이 출력

await를 사용하지 않았다면 데이터를 받아온 시점에 콘솔을 출력할 수 있게 콜백 함수나 .then()등을 사용해야 했을 겁니다.
하지만 async await 문법 덕택에 비동기에 대한 사고를 하지 않아도 됩니다.




