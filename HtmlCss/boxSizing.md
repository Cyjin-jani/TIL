## box-sizing 제대로 알고 사용하기
<br>

이미지 선택(클릭)시 테두리가 생기는데, 이 때문에 사이즈가 선택되지 않은 이미지들과 크기가 달라져서 화면이 덜컹거리는 이슈가 있었다.

이는 box-sizing에 대한 부분 때문이었는데, border-box로 되어있었기에 테두리영역을 width에 포함시키다 보니, 이미지가 조금 축소되어 버려서 덜컹거리는 느낌이 생기는 문제였다.

이를 해결하고자 box-sizing의 border-box와 content-box에 대해 알아 볼 필요가 있었다.

두 특성에 대한 설명은 다음과 같다.

[border-box의 특성]

- width와 height 속성이 안쪽 여백 과 테두리(border)영역을 포함한다.
- 즉, 아래와 같은 경우, width는 350px이다.
    
    ```css
    .box {
    	width: 350px;
      border: 10px solid black;
    }
    ```
    
- 요소의 크기 계산법
    - 너비 = 테두리(border) + 안쪽 여백(padding) + 콘텐츠 너비
    - 높이 = 테두리 + 안쪽 여백 + 콘텐츠 높이

[content-box의 특성]

- 기본 CSS 박스 크기 결정법을 사용
- width와 height 속성이 콘텐트 영역만 포함한다.
- border, margin 등의 속성은 width, height에 포함시켜 계산하지 않는다.
- 즉, 아래와 같은 css코드의 width는 370px이다.

```css
.box {
	width: 350px;
  border: 10px solid black;
}
```

결론

이미지가 덜컹거리는 문제점의 스무스한 해결은 box-sizing은 border-box로 둔 채, 보더를 1px 똑같이 적용한다.  그러나 보더의 색상은 다르게 적용하면, 이미지가 덜컹거리지 않고 테두리만 잘 표시가 된다.

```tsx
border: 1px solid transparent; // 혹은 #fff(흰색)

// 무언가 선택 되면 색상을 변경 (꼭 blue일 필요는 없다)
border: ${({ isSelected }) => isSelected && '1px solid blue' };
```

참고

[https://developer.mozilla.org/ko/docs/Web/CSS/box-sizing](https://developer.mozilla.org/ko/docs/Web/CSS/box-sizing)