## 버튼 클릭 시 테두리 효과 없애기
<br>
버튼 클릭 시 자연적으로 발생하는 outline 테두리 효과 없애는 방법.

버튼을 클릭할 때 생기는 효과는 focus, active의 속성을 바꾸면 해결이 가능하다.

```css
&:active,
  &:focus {
    outline: none;
  }
```

border: none; 을 추가해줘도 좋다.