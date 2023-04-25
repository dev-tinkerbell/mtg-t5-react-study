# 리액트는 어쩌다 만들어졌을까?

## 문제

웹의 복잡성이 늘어나, 인터랙션이 자주 발생하고 이에따라 동적으로 UI가 변경된 것을 브라우저에 표현하기위해 DOM을 직접 컨트롤 하기 어렵다.

## 해결안

- 자바스크립트의 특정 값이 바뀌면 특정 DOM의 속성이 바뀌도록 연결해주어 복잡성을 간소화 함

  ⇒ Ember, Backbone, Angular JS 프레임워크

  단점: 특정 속성을 어떻게 연결해서 업데이트 시켜줘야 할지에 대한 고민

- 특정 DOM과 특정 속성 연결 생각하지 말고 날려버리고 다시 만들자!

  ⇒ 리액트 프레임워크

  단점: 모든걸 날리고 새로 만들면 시간이 오래걸림

  해결안: Virtual DOM이라는 것을 사용

## 궁금증

- DOM이란?
- 브라우저와 DOM의 관계
- 브라우저 동작방식이 어떻게 되길래?
- Angular JS는 동작방식이 어떻게 이뤄지지?
- React의 virtual DOM이 어떻게 좋은건데?

## DOM이란?

- Document Object Model(문서 객체 모델)의 줄임말
- 자바스크립트가 직접 HTML을 동적으로 바꾸는데에는 한계가 있으니 JS가 조작하기 편하게 DOM이라는 형태로 바꾸었다.
- DOM은 nodes와 objects로 문서를 표현한다.
  ![이미지](https://velog.velcdn.com/images/yesbb/post/d1007877-f477-4993-b42e-1e7c27ebdfe4/image.png)

- DOM은 HTML, XML 문서에 접근 할 수 있는 방법을 제공하여 HTML, XML 구조 및 스타일 내용등을 변경하게 돕는다.

  ```js
  const element = document.getElementById("greeting");
  element.style.color = "blue";
  ```

## 브라우저와 DOM의 관계

- DOM을 기반으로 HTML 문서에 접근할수 있다.

- 브라우저를 동작시키기 위한 과정중 하나일 것이다 라는 추측

## 브라우저 동작방식이 어떻게 되길래?

- 브라우저마다 각기 다른 렌더링 엔진을 가지고 있다.

  크롬 - 웹킷, 모질라 - 게코

  ![이미지](https://d2.naver.com/content/images/2015/06/helloworld-59361-3.png)

## Angular JS는 동작방식이 어떻게 이뤄지지?

- MVC 구조를 제공 (Model, View, Controller)

![이미지](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Ft1.daumcdn.net%2Fcfile%2Ftistory%2F245F7F4354BE46FA0D)

1. 브라우저가 html을 로드 (DOM을 파싱한다.)
2. Angular.js를 로드한다
3. DOM Content Loaded Event를 기다린다.
4. DOM이 모두 로드되면 ng-app 지시자를 찾는다.
5. ng-app에서는 dependency injection 을 위해 사용되는 $injector를 생성한다.
6. injector 지시어는 어플리케이션의 모델을 위한 컨텍스트가 되는 루트 스코프를 생성한다.
7. 최종적으로 ng-app을 기준으로 하위DOM을 컴파일하고 rootScope와 링크시킨다.

결론: DOM을 더 쉽게 조작할 수 있고 로직과 뷰를 구별하여 코드 작성에 용이
단점: 용량이 크다.

## React의 virtual DOM이 어떻게 좋은건데?

virtual DOM은 메모리 상에 존재하는 하나의 객체이다.

![이미지](https://velog.velcdn.com/images/yesbb/post/43332f9c-1630-40b7-a1f7-a0325df77f8e/image.png)

1. real dom으로부터 virtual dom을 만든다

2. 변화가 생기면 새로운 버전의 virtual dom을 만든다.

3. old 버전의 virtual dom과 new 버전의 virtual dom을 비교한다.(diff algorithm)

4. 비교 과정을 통해서 발견한 차이점을 real dom에 적용한다.

### 그냥 변경된 DOM을 찾아서 기존 DOM에 업데이트 시키면 안되나?

변경된 DOM을 찾는 방법

- dirty checking
  - node tree를 재귀적으로 계속 순회하면서 어떤 노드에 변화가 생겼는지를 인식하는 방법
  - 변화가 없을 때도 재귀적으로 노드를 탐색함으로써 불필요한 비용이 발생
  - angular.js
- observable
  - 변화가 생긴 노드가 관찰자에게 알림을 보내주는 방식
  - 리액트의 state 변화 감지

결론: virtual DOM을 사용함으로써 변화된 부분만을 참고하여 저렴한 비용으로 업데이트 가능

## 참조

- [DOM MDN](https://developer.mozilla.org/ko/docs/Web/API/Document_Object_Model/Introduction)
- [브라우저 동작방식](https://d2.naver.com/helloworld/59361)
- [앵귤러 동작방식](https://heiswed.tistory.com/entry/AngularJS-%EC%86%8C%EA%B0%9C)
- [virtual-dom의-성능이-더-좋은이유](https://velog.io/@yesbb/virtual-dom%EC%9D%98-%EC%84%B1%EB%8A%A5%EC%9D%B4-%EB%8D%94-%EC%A2%8B%EC%9D%80%EC%9D%B4%EC%9C%A0)
