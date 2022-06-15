## 모바일 전용 가로스크롤 탭 구현하기
<br>

모바일 반응형 개발을 하다 보면, 탭 같은 경우 작은 화면에 비해 긴 탭 내용 때문에 ui가 깨지는 현상이 쉽게 일어난다.

예시)

![Untitled (3)](https://user-images.githubusercontent.com/57857367/173849891-321dcde5-86a8-4d5e-890a-ac4c82fcb38d.png)

이렇게 보기에 영 좋지 않은 탭 메뉴를 가로스크롤이 생기도록 하여 

아래와 같이 보기에 자연스럽게 만들어줄 수 있다.

![Untitled (4)](https://user-images.githubusercontent.com/57857367/173850374-ba8f881b-0405-4ef4-b943-ffa29666acd5.png)

이렇게 보다 모바일 친화적인 UI를 구현하는 방법은 css의 속성 중 white-space와 overflow를 이용하면 된다.

코드 예시는 다음과 같다.

```jsx
width: 100vw;
padding-left: 24px; // 이건 꼭 해줄 필요는 없다.. 단지 간격 맞추기 위함일 뿐.
overflow-x: auto;
white-space: nowrap;
```

white-space속성은 공백문자를 처리하는 방법을 지정하는 것인데, 사용할 수 있는 값은 다음과 같이 여러가지가 있다.

`white-space: normal | nowrap | pre | pre-wrap | pre-line | initial | inherit`

아무 설정을 하지 않은 default값이 normal인데, 이는 연속 공백을 하나로 합쳐준다. 

또한, 화면을 넘어가게 되는 요소가 있다면 자동으로 줄 바꿈을 해준다.

우리가 사용하는 nowrap은 역시 연속 공백을 하나로 합쳐주지만, <br>태그가 아니라면 줄바꿈을 허용하지 않는다. 그렇기에 화면 바깥으로 요소가 벗어나게 된다.

이렇게 블록 요소의 바깥으로 벗어난 요소를 컨트롤 하는 것이 바로 overflow 속성이다.

위에서는 요소가 x축으로만 overflow가 발생하는 것을 알고 있기에, 

보다 명시적으로  필요시 스크롤이 생기도록 하기 위해 overflow-x: auto값으로 지정했다.

결론적으로, white-space와 overflow 속성을 적절히 활용하면 모바일에서 가로스크롤 탭을 구현할 수 있다!

반응형 웹 개발시 은근 자주 사용되는 부분인 듯 하여 정리를 해 보았다.

참고

[https://gahyun-web-diary.tistory.com/206](https://gahyun-web-diary.tistory.com/206)

[https://developer.mozilla.org/ko/docs/Web/CSS/white-space](https://developer.mozilla.org/ko/docs/Web/CSS/white-space)