## ios기기에서 인풋이 자동으로 줌인 되는 문제 해결방법
<br>

input, textarea, selectbox 등에서 font-size가 16px 보다 작은 경우, ios에서는 인풋 등에 focus가 되면 친절하게 zoom-in을 해준다.

하지만 이미 모바일에 최적화 된 페이지인 경우 굳이 auto로 zoom-in이 되어야 하나 싶다는 생각이 들었지만, ios의 배려이기에 이 또한 UX를 충분히 고려해봐야한다는 생각도 같이 들었다.

이러나 저러나 회사의 프로젝트에서는 이미 디자인이 정해져서 font size를 16보다 작게 써야한다는 요구가 있었고, 자동으로 zoom-in이 되면 안된다는 요구사항을 받았다.

이 auto zoom-in을 막기 위한 방법으로는 크게 3가지가 존재한다.

1. 그냥 font-size를 최소 16px로 맞춘다.
2. meta tag에 줌 확대가 안되도록 한다.
3. scale을 이용한다.

우선, 필자는 2번을 활용하여 해당 문제점을 해결했는데, 이미 모바일 최적화 된 디자인이었으므로 유저가 확대를 할 필요성이 거의 없다고 판단되었기 때문이다. 

해당 설정을 위한 코드는 아래와 같다.

```tsx
<meta
	name="viewport"
  content="initial-scale=1.0, width=device-width, maximum-scale=1.0, user-scalable=0"
  key="viewport"
/>
```

그러나, 사실 생각해보면, 모바일 최적화 된 디자인일 지라도, 눈이 불편해서 줌인을 원하는 이용자는 분명 존재할 것이라 생각했고, 아래와 같은 scale을 활용하는게 더 나은 방향이 아닌가 싶다.

물론, css에 추가적인 코드들이 필요하며, 별도의 계산을 통해 이를 설정해줘야 해서 개발경험은 조금 떨어질 수는 있으나, scale을 이용한 것이 가장 유저친화적인 서비스라고 생각한다.

사용 방법은 다음과 같다.

최종적으로 적용하고 싶은 스타일

```jsx
input[type="text"] {
    border-radius: 5px;
    font-size: 12px;
    line-height: 20px;
    padding: 5px;
    width: 100%;
}
```

16px로 폰트 크기를 정하고 12px 크기로 보이도록 `<input>`을 축소할 것이기 때문에, 그 비율만큼 다른 요소들의 크기를 키운다.

```jsx
input[type="text"] {
    border-radius: 6.666666667px;
    font-size: 16px;
    line-height: 26.666666667px;
    padding: 6.666666667px;
    width: 133.333333333%;

		// 계산된 크기만큼 스케일을 해서 인풋의 크기를 줄여준다.
		transform: scale(0.75);
    transform-origin: left top;

		// 억지로 줄인만큼 아래 마진(공백)이 생기므로 공백을 제거한다.
		margin-bottom: -10px;
    margin-right: -33.333333333%;
}
```

다만, 위 경우에, input에 다양한 요소가 쓰이고 있는 경우 (예를 들면 suffix...라던가....)

위 코드를 적용시키는 데 무리가 있을 수 있다.

가장 좋은 것은 최소 크기를 16px로 염두하고 디자인을 하는 것일테지만, 그게 불가능한 경우 메타태그를 적용하는 것을 고려해봐야 한다고 생각한다.

회사 프젝에서는 가장 시간적으로 비용이 적게 드는 메타 태그 설정을 추가하는 방법을 적용시켰다.

참고

[https://velog.io/@awesomelon/JS-iOS-input-zoom-막기](https://velog.io/@awesomelon/JS-iOS-input-zoom-%EB%A7%89%EA%B8%B0)

[https://devsoyoung.github.io/posts/ios-input-focus-zoom/](https://devsoyoung.github.io/posts/ios-input-focus-zoom/)