## styled Component에서 선택자 활용하기
<br>
기존의 css에서는 선택자를 다양하게 활용해서 스타일링을 해 왔다.
<br>
이를 테면 아래와 같은 방식의 코드들도 볼 수 있었는데...아래와 같은 경우를 만나면 일단 머리부터 지끈거리게 된다. <br>(그래서 이게 어떤 element, 어떤 컴포넌트인데?)

```tsx
div > div > div {
	background: red;
}
```

물론 styled-component에서도 못 할 건 없지만,<br>

좀 더 스타일링 유지보수를 쉽게 하기 위해<br> 아래와 같이 **${styledComponet변수명}** 활용한다.

예시) - 라디오 버튼 예시


```tsx
const HtmlRadio = styled.input`
  position: absolute;
  opacity: 0;
  top: 0;
  left: 0;
`;
const RadioMark = styled.div`
  width: 0;
  height: 0;
  border-radius: 50%;
`;

const StyledRadioBox = styled.label`
  display: flex;
  align-items: center;
  justify-content: center;
  position: relative;
  border-radius: 50%;
  cursor: pointer;

  ${HtmlRadio}:checked ~ & {
    background-color: #fff;

    & > ${RadioMark} {
      background-color: ${({ theme }) => theme.colors.dark};
    }
  }

  ${HtmlRadio}:disabled ~ & {
    border-color: transparent;
    background-color: ${({ theme }) => theme.colors.background};
  }

  ${HtmlRadio}:checked:disabled ~ & {
    background-color: #fff;
    border-color: ${({ theme }) => theme.colors.borderLighten};

    & > ${RadioMark} {
      background-color: ${({ theme }) => theme.colors.gray70};
    }
  }
`;
```