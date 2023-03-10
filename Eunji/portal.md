# React Portal

> 부모 컴포넌트의 DOM 계층 구조 바깥에 있는 DOM 노드로 자식을 렌더링하는 최고의 방법

<br>

```javascript
ReactDOM.createPortal(child, targetElement);
```

- 첫 번째 인자 : 엘리먼트, 문자열, 혹은 fragment와 같은 어떤 종류이든 렌더링할 수 있는 값
- 두 번째 인자 : DOM 엘리먼트

createPortal을 활용해 child를 targetElement에 렌더링

<br>

## portal을 사용하는 이유

모달을 구현할 때 다른 부모 엘리먼트와 사용하면 css 구조로 인해 부모 엘리먼트 스타일의 영향을 받을 수 있다.
이 경우 레이아웃이 깨지는 경우가 발생할 수 있는데 이런 상황을 해결하기 위해 리액트 포탈을 사용한다.

리액트 포탈을 활용하면 특정 엘리먼트를 어플리케이션이 종속 되어 있는 돔 트리를 벗어나 외부의 다른 돔으로 렌더할 수 있다.

<br>

## portal의 이벤트

portal 내부에서 발생한 이벤트는 실제 렌더링 위치가 다르지만 부모 component의 하위 dom처럼 동작한다 (버블링, 캡쳐링 발생)

DOM 트리에서의 위치에 상관없이 portal은 여전히 React 트리에 존재하기 때문이다.

포탈로 만든 앨리먼트는 실제 돔 구조와 달리 리액트 어플리케이션 컴포넌트 트리 구조를 따른다.

<br>

## portal과 render의 차이점

공통점 : 특정 DOM으로 컴포넌트를 렌더링

- Portal의 LifeCycle은 호출한 부모 컴포넌트에 종속<br>
- Render는 새로운 React LifeCycle을 생성<br>
  => React App이나 Notification등 독자적인 컴포넌트 렌더링에 사용한다
