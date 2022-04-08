## 리스트 태그 CSS 정의 & 활용하기

<br>

기본적으로 ol, ul 태그는 각 본연의 스타일을 가지고 있지만, {list-style} 속성을 이용하여 리스트의 스타일을 정의할 수 있다.

사용예시는 다음과 같다.

```jsx
list-style-position: outside; // 리스트의 내용이 긴 경우 들여쓰기가 적용됩니다.
list-style-type: decimal; // 숫자(10진수)가 나옵니다.
```

세부 속성 이외에 list-style을 전체적으로 설정할 수도 있다.

```jsx
/* type */
list-style: square;

/* image */
list-style: url('../img/shape.png');

/* position */
list-style: inside;

/* type | position */
list-style: georgian inside;

/* type | image | position */
list-style: lower-roman url('../img/shape.png') outside;

/* Keyword value */
list-style: none;

/* Global values */
list-style: inherit;
list-style: initial;
list-style: revert;
list-style: unset;
```

세부 속성 중 list-style-type은 자유롭게 커스터마이징이 가능하다.

```jsx
/* Partial list of types */
list-style-type: disc;
list-style-type: circle;
list-style-type: square;
list-style-type: decimal;
list-style-type: georgian;
list-style-type: trad-chinese-informal;
list-style-type: kannada;

/* <string> value */
list-style-type: '-';

/* Identifier matching an @counter-style rule */
list-style-type: custom-counter-style;

/* Keyword value */
list-style-type: none;

/* Global values */
list-style-type: inherit;
list-style-type: initial;
list-style-type: revert;
list-style-type: unset;
```

출처

[https://developer.mozilla.org/en-US/docs/Web/CSS/list-style](https://developer.mozilla.org/en-US/docs/Web/CSS/list-style)