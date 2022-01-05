## user-select 활용하기
<br>
user-select란?

사용자가 텍스트를 선택할 수 있는지 지정할 때 사용하는 css요소이다.

키워드/전역 값은 다음과 같다.

```tsx
/* 키워드 값 */
user-select: none;
user-select: auto;
user-select: text;
user-select: contain;
user-select: all;

/* 전역 값 */
user-select: inherit;
user-select: initial;
user-select: unset;
```

여러 브라우저에서 해당 텍스트를 선택되지 않도록 하려면 아래와 같이 사용하면 된다.

```tsx
  -moz-user-select: none;
  -webkit-user-select: none;
  -ms-user-select: none;
  user-select: none;
```

사용처:

button, checkbox의 라벨, radioBtn의 라벨, dropDown, tab 등...

클릭 이벤트가 있는 경우에는 해당 text를 복사하거나 그런 기능을 굳이 줄 필요가 없어서 막는게 더 맞지 않나 싶다.

무엇이 좋고 나쁜지는 결국 UX/UI의 설계에 따라 달라질 듯 하지만,
기본적으로 무언가 클릭 이벤트를 발생하는 요소라면 text가 선택이 되지 않도록 하는 게 맞는 것 같다.

(안그러면 클릭 이벤트 대신 텍스트 블록을 해버리는 예상치 못한 UX가 될 수 있으니!)