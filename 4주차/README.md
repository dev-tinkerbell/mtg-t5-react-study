# ECMAScript6 === ES6

## 목차

- [let, const와 블록 레벨 스코프](/4%EC%A3%BC%EC%B0%A8#let-const%EC%99%80-%EB%B8%94%EB%A1%9D-%EB%A0%88%EB%B2%A8-%EC%8A%A4%EC%BD%94%ED%94%84)
- [템플릿 리터럴](#템플릿-리터럴)
- [화살표 함수]()
- [매개변수 기본값, Rest 파라미터, Spread 문법, Rest/Spread 프로퍼티](/es6-extended-parameter-handling)
- [객체 리터럴 프로퍼티 기능 확장]()
- [디스트럭처링]()

## let, const와 블록 레벨 스코프

### 기존 var의 문제

1. 함수 레벨 스코프

   따라서 전역 함수 외부에서 생성한 변수는 모두 전역 변수이다.

   ```jsx
   function foo() {
     // 함수 레벨 스코프
   }

   for (let i = 0; i < arr.length; i++) {
     // 블록 레벨 스코프
   }
   ```

2. var 키워드 생략 허용

   ```jsx
   apple = "사과";
   ```

3. 변수 중복 선언 허용

   ```jsx
   var apple = "사과";
   var apple = "바나나";
   ```

4. 변수 호이스팅

   ```jsx
   console.log(apple); // '사과'
   var apple = "사과";
   ```

⇒ 전역 변수는 유효 범위가 넓어서 어디서 어떻게 사용되어야 할지 파악하기 힘들다. 즉, 변수의 스코프는 좁을수록 좋다.

### let

1. 블록 레벨 스코프
2. 변수 중복 선언 금지

   ```jsx
   let bar = 123;
   let bar = 456; // Uncaught SyntaxError: Identifier 'bar' has already been declared
   ```

3. 호이스팅

   ```jsx
   console.log(bar); // Error: Uncaught ReferenceError: bar is not defined
   let bar;
   ```

### const

1. 블록 레벨 스코프
2. 재할당 금지

   ```jsx
   const FOO = 123;
   FOO = 456; // TypeError: Assignment to constant variable.
   ```

---

## 템플릿 리터럴

- 백틱(backtick) 문자 ```를 사용
- `‘` 또는 `“` 사용 가능
- 줄바꿈, 공백 허용
- `+` 연산자를 사용하지 않고 문자열 인터폴레이션(String Interpolation)으로 문자열을 삽입할수 있다.

```jsx
const first = "Ung-mo";
const last = "Lee";

// ES5: 문자열 연결
console.log("My name is " + first + " " + last + ".");
// "My name is Ung-mo Lee."

// ES6: String Interpolation
console.log(`My name is ${first} ${last}.`);
// "My name is Ung-mo Lee."
```

### Tagged Template Literal

- 템플릿 리터럴의 발전된 형태로서 함수 형태로 사용 가능
- 텍스트와 자바스크립트 값 구분

  ```jsx
  const red = "빨간색";
  const blue = "파란색";

  function favoriteColors(texts, ...values) {
    console.log(texts);
    console.log(values);
  }

  favoriteColors`제가 좋아하는 색은 ${red}과 ${blue}입니다.`;
  ```

  ![https://i.imgur.com/869yKxk.png](https://i.imgur.com/869yKxk.png)

## 화살표 함수

### 사용방법

```jsx
// 매개변수 지정 방법
    () => { ... } // 매개변수가 없을 경우
     x => { ... } // 매개변수가 한 개인 경우, 소괄호를 생략할 수 있다.
(x, y) => { ... } // 매개변수가 여러 개인 경우, 소괄호를 생략할 수 없다.

// 함수 몸체 지정 방법
x => { return x * x }  // single line block
x => x * x             // 함수 몸체가 한줄의 구문이라면 중괄호를 생략할 수 있으며 암묵적으로 return된다. 위 표현과 동일하다.

() => { return { a: 1 }; }
() => ({ a: 1 })  // 위 표현과 동일하다. 객체 반환시 소괄호를 사용한다.

() => {           // multi line block.
  const x = 10;
  return x * x;
};
```

### 화살표 함수 호출 방법

화살표 함수는 익명 함수로만 사용할 수 있다. 따라서 화살표 함수를 호출하기 위해서는 함수 표현식을 사용

```jsx
// ES5
var pow = function (x) {
  return x * x;
};
console.log(pow(10)); // 100

// ES6
const pow = (x) => x * x;
console.log(pow(10)); // 100
```

---

## 매개변수 기본값, Rest 파라미터, Spread 문법, Rest/Spread 프로퍼티

### 매개변수 기본값

```jsx
function sum(x, y) {
  return x + y;
}

console.log(sum(1)); // NaN

// 매개변수 기본값
function sum(x = 0, y = 0) {
  return x + y;
}

console.log(sum(1)); // 1
```

### Rest 파라미터

- 매개변수 이름 앞에 세개의 점 `...`을 붙여서 정의한 매개변수
- Rest 파라미터는 함수에 전달된 인수들의 목록을 배열로 전달받는다.

  ```jsx
  function foo(...rest) {
    console.log(Array.isArray(rest)); // true
    console.log(rest); // [ 1, 2, 3, 4, 5 ]
  }

  foo(1, 2, 3, 4, 5);
  ```

- 함수에 전달된 인수들은 순차적으로 파라미터와 Rest 파라미터에 할당된다.

  ```jsx
  function bar(param1, param2, ...rest) {
    console.log(param1); // 1
    console.log(param2); // 2
    console.log(rest); // [ 3, 4, 5 ]
  }

  bar(1, 2, 3, 4, 5);
  ```

### Spread 문법

- Spread 문법의 대상은 [이터러블](https://poiemaweb.com/es6-iteration-for-of)이고 Spread 문법(Spread Syntax, `...`)는 대상을 개별 요소로 분리
  ```jsx
  // ...[1, 2, 3]는 [1, 2, 3]을 개별 요소로 분리한다(→ 1, 2, 3)
  console.log(...[1, 2, 3]); // 1, 2, 3
  ```
- Rest 파라미터와 Spread 문법은 형태가 동일하여 혼동할수 있어 주의!

  ```jsx
  /* Spread 문법을 사용한 매개변수 정의 (= Rest 파라미터)
     ...rest는 분리된 요소들을 함수 내부에 배열로 전달한다. */
  function foo(param, ...rest) {
    console.log(param); // 1
    console.log(rest); // [ 2, 3 ]
  }
  foo(1, 2, 3);

  /* Spread 문법을 사용한 인수
    배열 인수는 분리되어 순차적으로 매개변수에 할당 */
  function bar(x, y, z) {
    console.log(x); // 1
    console.log(y); // 2
    console.log(z); // 3
  }

  // ...[1, 2, 3]는 [1, 2, 3]을 개별 요소로 분리한다(-> 1, 2, 3)
  // spread 문법에 의해 분리된 배열의 요소는 개별적인 인자로서 각각의 매개변수에 전달된다.
  bar(...[1, 2, 3]);
  ```

- 🌟 배열에서 사용하는 경우
  기존 배열의 요소를 새로운 배열 요소의 일부로 만들고 싶은 경우

  ```jsx
  // ES5
  var arr = [1, 2, 3];
  console.log(arr.concat([4, 5, 6])); // [ 1, 2, 3, 4, 5, 6 ]

  // ES6
  const arr = [1, 2, 3];
  // ...arr은 [1, 2, 3]을 개별 요소로 분리한다
  console.log([...arr, 4, 5, 6]); // [ 1, 2, 3, 4, 5, 6 ]
  ```

- 특정 프로퍼티 변경 / 추가

  ```jsx
  // 특정 프로퍼티 변경
  const changed = { ...{ x: 1, y: 2 }, y: 100 };
  // changed = { ...{ x: 1, y: 2 }, ...{ y: 100 } }
  console.log(changed); // { x: 1, y: 100 }

  // 프로퍼티 추가
  const added = { ...{ x: 1, y: 2 }, z: 0 };
  // added = { ...{ x: 1, y: 2 }, ...{ z: 0 } }
  console.log(added); // { x: 1, y: 2, z: 0 }
  ```

---

## 객체 리터럴 프로퍼티 기능 확장

### 프로퍼티 축약 표현

```jsx
// ES5
var x = 1,
  y = 2;

var obj = {
  x: x,
  y: y,
};

console.log(obj); // { x: 1, y: 2 }

// ES6
let x = 1,
  y = 2;

const obj = { x, y };

console.log(obj); // { x: 1, y: 2 }
```

### 프로퍼티 키 동적 생성

프로퍼티 키로 사용할 표현식을 대괄호([…])로 묶어야한다.

이를 계산된 프로퍼티 이름(Computed property name)라고 한다.

```jsx
// ES5
var prefix = "prop";
var i = 0;

var obj = {};

obj[prefix + "-" + ++i] = i;
obj[prefix + "-" + ++i] = i;
obj[prefix + "-" + ++i] = i;

console.log(obj); // {prop-1: 1, prop-2: 2, prop-3: 3}

// ES6
const prefix = "prop";
let i = 0;

const obj = {
  [`${prefix}-${++i}`]: i,
  [`${prefix}-${++i}`]: i,
  [`${prefix}-${++i}`]: i,
};

console.log(obj); // {prop-1: 1, prop-2: 2, prop-3: 3}
```

### 메소드 축약 표현

```jsx
// ES5 - 프로퍼티 값으로 함수 선언식을 할당
var obj = {
  name: "Lee",
  sayHi: function () {
    console.log("Hi! " + this.name);
  },
};

obj.sayHi(); // Hi! Lee

// ES6 - function 키워드를 생략한 축약 표현 사용가능
const obj = {
  name: "Lee",
  // 메소드 축약 표현
  sayHi() {
    console.log("Hi! " + this.name);
  },
};

obj.sayHi(); // Hi! Lee
```

---

## 디스트럭처링 (구조분해할당)

### 배열

```jsx
// ES5
var arr = [1, 2, 3];

var one = arr[0];
var two = arr[1];
var three = arr[2];

console.log(one, two, three); // 1 2 3

// ES6 Destructuring
const arr = [1, 2, 3];

// 배열의 인덱스를 기준으로 배열로부터 요소를 추출하여 변수에 할당
// 변수 one, two, three가 선언되고 arr(initializer(초기화자))가 Destructuring(비구조화, 파괴)되어 할당된다.
const [one, two, three] = arr;
// 디스트럭처링을 사용할 때는 반드시 initializer(초기화자)를 할당해야 한다.
// const [one, two, three]; // SyntaxError: Missing initializer in destructuring declaration

console.log(one, two, three); // 1 2 3
```

### 객체

```jsx
// ES5
var obj = { firstName: "Ungmo", lastName: "Lee" };

var _firstName = obj.firstName;
var _lastName = obj.lastName;

console.log(_firstName, _lastName); // Ungmo Lee

// ES6 Destructuring
const obj = { firstName: "Ungmo", lastName: "Lee" };

// 프로퍼티 키를 기준으로 디스트럭처링 할당이 이루어진다. 순서는 의미가 없다.
// 변수 lastName, firstName가 선언되고 obj(initializer(초기화자))가 Destructuring(비구조화, 파괴)되어 할당된다.
const { lastName, firstName } = obj;

console.log(firstName, lastName); // Ungmo Lee
```

# 참조

- [let, const와 블록 레벨 스코프](https://poiemaweb.com/es6-block-scope)
- [템플릿 리터럴](https://poiemaweb.com/es6-template-literals)
- [Tagged Template Literals 문법 :: 마이구미](https://mygumi.tistory.com/395)
- [화살표 함수](https://poiemaweb.com/es6-arrow-function)
- [매개변수 기본값, Rest 파라미터, Spread 문법, Rest/Spread 프로퍼티](https://poiemaweb.com/es6-extended-parameter-handling)
- [객체 리터럴 프로퍼티 기능 확장](https://poiemaweb.com/es6-enhanced-object-property)
- [디스트럭처링](https://poiemaweb.com/es6-destructuring)
