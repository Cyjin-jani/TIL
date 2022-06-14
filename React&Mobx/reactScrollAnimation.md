## React에서 스크롤 이벤트 구현하기
<br>

리액트(next.js)에서 스크롤 애니메이션 구현 및 적용하기

App.page

```jsx
// app.page.jsx 에 추가된 부분.
useEffect(() => {
    if (window) {
      const saTriggerMargin = 300;
      const saElementList = document.querySelectorAll('.sa');

      const saFunc = function () {
        for (const element of saElementList) {
          if (!element.classList.contains('show')) {
            if (
              window.innerHeight >
              element.getBoundingClientRect().top + saTriggerMargin
            ) {
              element.classList.add('show');
            }
          }
        }
      };

      window.addEventListener('load', saFunc);
      window.addEventListener('scroll', saFunc);
    }
  }, []);
```

참고: for - of는 ie에서 대응이 안된다만... 바벨을 이용하면 될수도 있다.

css - global

```css
/* Scroll Animation (sa, 스크롤 애니메이션) */
.sa {
      opacity: 0;
      transition: all .5s ease-out;
    }
/* 아래에서 위로 페이드 인 */
.sa-up {
  transform: translate(0, 100px);
}
.sa.show {
  opacity: 1;
  transform: none;
}
```

사용되는 페이지에서 적용하기 

```jsx
// page.jsx - 사용하고 싶은 섹션에 className을 넣어준다.
<Container className="sa sa-up">
  <Typography variant="h400">form 없음</Typography>
</Container>
```

참고

[https://joshua-dev-story.blogspot.com/2020/11/javascript-css-scroll-animation.html](https://joshua-dev-story.blogspot.com/2020/11/javascript-css-scroll-animation.html)

추가자료 - 만약 컴포넌트화 한다면...?

[https://velog.io/@syoung125/웹에서-애니메이션을-다뤄보자-2](https://velog.io/@syoung125/%EC%9B%B9%EC%97%90%EC%84%9C-%EC%95%A0%EB%8B%88%EB%A9%94%EC%9D%B4%EC%85%98%EC%9D%84-%EB%8B%A4%EB%A4%84%EB%B3%B4%EC%9E%90-2)

[https://medium.com/@shlee1353/리액트-styled-components-애니메이션-구현-fbbb8aa9e722](https://medium.com/@shlee1353/%EB%A6%AC%EC%95%A1%ED%8A%B8-styled-components-%EC%95%A0%EB%8B%88%EB%A9%94%EC%9D%B4%EC%85%98-%EA%B5%AC%ED%98%84-fbbb8aa9e722)