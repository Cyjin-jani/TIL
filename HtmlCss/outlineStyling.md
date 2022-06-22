## css outline 스타일링하기
<br>

focus 혹은 hover 시,
input이나 textArea 등에서 주로 outline의 커스텀 스타일링이 요구되는 경우가 많다.

기본적으로는 outline을 none으로 가져가고 border 스타일을 변형시키는 경우도 존재하나

outline 자체의 스타일링을 바꾸는 방법 또한 존재한다.

outline의 스타일링에는 크게 3가지 속성이 존재한다.

outline-style

outline-width

outline-color

style과 width 속성에선 이미 css에서 정해준 값들이 존재하며, width같은 경우에는 단위가 있는 값도 사용이 가능하다.

사전에 정의된 값은 다음과 같다.

outline-style의 경우

```jsx
/* 키워드 값 */
outline-style: auto;
outline-style: none;
outline-style: dotted;
outline-style: dashed;
outline-style: solid;
outline-style: double;
outline-style: groove;
outline-style: ridge;
outline-style: inset;
outline-style: outset;

/* 전역 값 */
outline-style: inherit;
outline-style: initial;
outline-style: unset;
```

outline-width의 경우는 다음과 같다.

```jsx
/* 키워드 값 */
outline-width: thin;
outline-width: medium;
outline-width: thick;

/* <length> 값 */
outline-width: 1px;
outline-width: 0.1em;

/* 전역 값 */
outline-width: inherit;
```

참고

[https://developer.mozilla.org/ko/docs/Web/CSS/outline](https://developer.mozilla.org/ko/docs/Web/CSS/outline)