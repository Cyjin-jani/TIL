## IOS에서 Input, textArea 박스 inner shadow 없애기
<br>

ios 폰 크롬의 경우, input, textarea 에 inner shadow 가 생기는 현상이 있었다.

해결 방법은 아래와 같다.

```tsx
input[type=text] {   
    /* Remove First */
    -webkit-appearance: none;
    -moz-appearance: none;
    appearance: none;

    /* Then Style */
    border-radius: 15px;
    border: 1px dashed #BBB;
    padding: 10px;
    line-height: 20px;
    text-align: center;
    background: transparent;
    outline: none;    
}
```

appearance를 먼저 지워두고 시작해야 한다는게 주의할 점이다.