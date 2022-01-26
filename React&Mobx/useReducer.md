## useReducer에 대해 간단 정리
<br>

리액트 공식 문서를 보면 useReducer는 useState를  대체할 수 있는 함수이다. 

아래와 같은 형태로 사용이 가능하다.

`const [state, dispatch] = useReducer(reducer, initialState);`

useReducer가 받는 인자값인 reducer에는, `(state, action) => newState`의 형태로 reducer를 받고 

`dispatch` 메서드와 짝의 형태로 현재 state를 반환한다.

또한, initialState를 2번째 인자 값에 넣어서 초기 값 세팅을 해줄 수 있다.

공식 문서에서는 다음과 같이 활용법을 소개하고 있다.

```tsx
function reducer(state, action) {
  switch (action.type) {
    case 'increment':
      return {count: state.count + 1};
    case 'decrement':
      return {count: state.count - 1};
    default:
      throw new Error();
  }
}

const countState = {count: 0};

function Counter() {
  const [state, dispatch] = useReducer(reducer, countState);
  return (
    <>
      Count: {state.count}
      <button onClick={() => dispatch({type: 'decrement'})}>-</button>
      <button onClick={() => dispatch({type: 'increment'})}>+</button>
    </>
  );
}
```

위와 같이 컴포넌트 안에서 동작하는 initialState를 만들어도 되고, global하게 쓰이는 context를 initialState자리에 넣어주어도 된다.

좀 더 자세히 알아보자.

useRedcuer는 **1. reducer라고 불리는 콜백함수 와 2. initialState 인자, 3. initializer 이라고 불리는 콜백함수를 받고, state와 dispatch를 반환한다.** 마지막 인자인 initializer는 optional한 인자이다.

reducer 함수는 state와 action을 인자로 받는다. 

action 객체가 dispatch되면 redcuer 함수에 state와 action객체가 매개변수로 실행되고, switch문으로 action.type을 기준으로 분기해서 현재 상태를 업데이트한 후 반환한다.
물론, action 객체에 type 이외에 다른 key를 설정하여 reducer에서 사용할 수 있다.

initialState는 초기 상태인데, 어떤 자료형이라도 올 수 있다. 보통은 boolean이나 string 등의 원시타입 보다는 객체 형태를 많이 사용한다. (그렇지 않으면 useState를 써도 될 것이다.)

initializer는 initialState를 인자로 받아 반환한 값이 initialState가 된다. React 공식문서에서 initializer에 대해 설명하기로는 "초기화 지연" 이라고 한다.initialState에 추가적인 가공이 필요할 때에 사용할만한 옵션이라고 생각하면 될 듯하다.

위에 나와있는 예시는 간단한 예시이며, 구조가 복잡한 객체(혹은 여러 가지 state)를 상태로 가지고 있고 setter 함수의 내용이 복잡하다면 useState보다는 useReducer를 사용해보자.

[https://ko.reactjs.org/docs/hooks-reference.html#usereducer](https://ko.reactjs.org/docs/hooks-reference.html#usereducer)

[https://velog.io/@code-bebop/useReducer에-대해-알아보자](https://velog.io/@code-bebop/useReducer%EC%97%90-%EB%8C%80%ED%95%B4-%EC%95%8C%EC%95%84%EB%B3%B4%EC%9E%90)