## IOS 기기에서 input disabled가 흐릿하게 나오는 현상 해결법
<br>

커스텀 input을 만들었는데, 일반 pc 브라우저 (window, mac) 및 안드로이드 폰에서는 

input이 disabled인 상태에서 글자가 또렷하게 잘 나왔다.

그러나, ios의 경우에는 매우 흐릿하게 글자가 보여서, 거의 알아보기 어려운 수준이었다.

![image](https://user-images.githubusercontent.com/57857367/159931805-fd44bee1-3d7c-4ead-aee0-c26549cf560b.png)

ios 모바일 기기에서만 발생하고 있어 쉽게 원인을 찾을 수 없었는데,

아래 글을 보고 해답을 얻었다.

[https://stackoverflow.com/questions/262158/disabled-input-text-color-on-ios](https://stackoverflow.com/questions/262158/disabled-input-text-color-on-ios)

코드에 적용하는 방법은 다음과 같다.

```tsx
${({ disabled, $textColor }) =>
    disabled &&
    css`
      &:disabled {
        -webkit-text-fill-color: ${({ theme }) =>
          $textColor ? $textColor : theme.colors.neutral[700]};
        opacity: 1;
      }
    `};
```

커스텀 input의 일부분인데, 핵심은 두 속성의 추가이다.

```css
-webkit-text-fill-color: {yourColor};
opacity: 1;
```

input 뿐 아니라 textarea에도 동일하게 적용해주면 된다.