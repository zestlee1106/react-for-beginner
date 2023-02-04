# 리액트를 사용해 봅시다

(with 노마드코더 강의)

리액트의 기초부터 차근차근 배워 보자

- 목표

1. 리액트의 구현 방법에 대해서 알아 보자
2. node 없이 어떻게 실행이 가능한지 알아 보자

---

## 리액트를 사용하는 방법

- 리액트는 node 가 있어야만 사용할 수 있다 라고 생각할 수 있지만, 아니다.
- 리액트는 그냥 라이브러리이기 때문에 import 만 해 주면 가능하다.
- index.html 을 하나 만들고, `react.production.min.js`, `react-dom.production.min.js`, `babel.min.js` 세 파일만 import 하면 된다.

### `react.production.min.js` 는 무엇인가?

- 요건 리액트를 사용할 수 있게끔 하는 js 이다

### `react-dom.production.min.js` 은 무엇인가?

- react dom 을 렌더링할 수 있게 해 준다

### `babel.min.js` 은 무엇인가?

- 많이 사용했던 바벨이 맞다.
- 리액트는 jsx (js 에서 html 을 이용)방식을 사용하는데, 해당 문법을 사용하기 위해서는 바벨이 필요하다.

---

## setState 사용하기

- 변수를 바꾼 후 렌더링을 다시 시키기 위해서는 render 함수를 다시 호출해 줘야 하는데, 반응형 변수를 사용하면 그렇게 할 필요가 없다.
- 반응형 변수를 만들기 위해서는 react 에서 제공해주는 setState 를 사용하면 된다.
- modifier 함수를 이용할 땐 그 안에서 state 를 직접 바꿔도 되지만, 안에서 함수형으로 쓰는 것이 현재 state 를 보장할 수 있기 때문에, 함수형으로 쓰는 것을 더 권장한다.

예시

```javascript
// ❌❌❌ 이렇게 하면 어떤 counter 를 바꾸는 건지 명확하지 않음
setCounter(counter + 1);
// 👍👍👍 이렇게 하면 변경할 state 를 보장할 수 있음
setCounter((current) => current + 1);
```

---

## jsx 에서 html 속성 사용하기

- jsx 내부에서 html 속성을 사용하려면 어떻게 해야 할까?
- 그냥 쓰면 되지 않나 라고 생각할 수 있지만, jsx 는 js 인 것을 명심해야 한다
- for -> htmlFor, class -> className 처럼 특정 속성명을 달아서 사용해야 한다

---

## value 단방향 바인딩 하기

- react 에서 input 에 값을 바인딩 할 땐 단방향 바인딩밖에 없다.
- 따라서 value 에 값을 바인딩하고, onChange 로 이벤트 핸들러를 연결하고, 그 핸들러 안에서 값을 또 변경해 줘야 한다
- value 에 값을 연결하는 것은 보여주기밖에 안한다
- dom 속성에 {} 를 바인딩하면 javascript 문법을 바인딩할 수 있다

```javascript
<input
  value={flipped ? amount * 60 : amount}
  id="minutes"
  onChange={onChange}
  disabled={flipped === true}
/>
```

---

## props 사용하여 재사용성 늘리기

- props 를 사용하면 재사용성을 늘릴 수 있다
- html 에서 속성을 추가한 것처럼 추가해 주면, jsx 가 읽으면서 함수에 인자를 넣어서 호출하는 것처럼 된다
- 컴포넌트 함수는 항상 props 를 가지고 있는데, 객체 형태라서 그걸 벗겨내면 props 값들을 가져다 쓸 수 있다
- 커스텀 컴포넌트에 html 의 속성처럼 생긴 props 를 넘기더라도 이벤트로 동작하지 않는다.
  - 예를 들어, style 이나 onClick 등을 넣었다면 dom 의 속성처럼 동작하지 않고, 그것들은 커스텀 컴포넌트의 props 일 뿐이다.
  - 따라서 자식 컴포넌트에서 props 를 받아서 특정 dom 에 넘겨주는 일을 해야 한다.
- 자식 컴포넌트에서 이벤트를 발생시켜서 부모의 state가 바뀌었을 경우 부모의 컴포넌트 모두가 re render 된다
  - 이것은 매우 좋은 기능이지만, 너무 자주 발생할 경우 애플리케이션이 느려질 수가 있다
  - 따라서 memorize 하는 기능인 React 의 memo 를 써서 props 가 바뀌지 않을 경우에는 re render 하지 말아달라고 정의할 수 있다

---

## propType 추가

- 코워커가 있다면 props 를 추가할 때 타입이 맞지 않는 prop 을 넘기려고 할 수도 있다
- 그럴 때 에러를 표출하기 위해서 propsType 이라는 게 추가되었다
- `prop-types.js` 를 import 하고 가져다 쓰기만 하면 된다
- 추가로, type 뿐만이 아닌, isRequired 를 추가하여 필수로 검사할 수도 있다
