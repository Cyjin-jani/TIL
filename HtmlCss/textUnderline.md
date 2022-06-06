## 텍스트 언더라인에 관해
<br>
text-decoration: underline을 많이 쓰곤 한다.

물론, 보통의 경우엔 underline보다는 none을 더 많이 사용한다. (커스텀 a태그 쓰는 경우)

밑줄의 경우, text-decoration: underline을 쓰면 가끔 너무 밑줄이 글자와 가까이 붙는 경우를 본 적이 있을 것이다.

이를 해결해주는 속성이 바로, text-underline-position이다.

키워드 및 사용법은 다음과 같다.

키워드

```css
/* Single keyword */
text-underline-position: auto;
text-underline-position: under;
text-underline-position: left;
text-underline-position: right;
```

사용법

```jsx
.under-below {
	text-decoration: underline;
  text-underline-position: under;
  -webkit-text-underline-position: under;
  -ms-text-underline-position: below; // ie대응
}
```

이 속성은 canIUse에서 살펴보아도 되겠지만,

ie, safari, firefox 등 모든 브라우저에서 동작한다.

다만, ie에서는 해당 속성의 값으로 under 대신에 below를 써주어야 한다. (under는 동작 안함)

참고

[https://developer.mozilla.org/en-US/docs/Web/CSS/text-underline-position](https://developer.mozilla.org/en-US/docs/Web/CSS/text-underline-position)