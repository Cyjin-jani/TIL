## flex로 grid처럼 리스트 만들어보기
<br>

순수하게 flex-box로 그리드와 비슷하게 구현하는 방법이 있다.

flex-wrap: wrap과 음수 마진을 적절히 사용하고, 그리드 요소의 width를 비율로 지정하고 padding으로 여백을 조절하면 그리드 라이브러리를 사용하지 않더라도 비슷하게 구현이 가능하다.

물론, 그냥 그리드 라이브러리를 사용하는게 더 편리할 수도 있다.

라이브러리에서는 xs, sm, md, lg 등 화면 사이즈별로 한 줄에 몇 개씩 보여줘야 하는지 손쉽게 설정이 가능하나,

flex-box 속성의 css만으로 구현해야 한다면, 미디어쿼리를 각 화면 사이즈 단계별로 조정을 하나 하나 다 해주어야 할 것이다.

거두절미하고 사용한 코드는 다음과 같다.

음수 마진과 list 요소에 패딩을 사용하였다.

```jsx
const SomethingListWrapper = styled.ol`
   display: flex;
   flex-direction: row;
   flex-wrap: wrap;
   margin: 40px -12px 80px -12px;
`;

const SomethingListItem = styled.li`
   display: block;
   width: 33.333%;
   padding: 0 12px;

   &:nth-child(n + 4) {
     margin-top: 32px;
   }
`;
```

물론 ol, li 등의 태그의 기본 속성값들은 아래와 같이 초기화를 한 후 진행한다.
(polished 라이브러리의 normalize 를 우선 적용한 뒤, 아래와 같이 추가적으로 속성 값 override함)

```css
li, ul, ol, dl {
  list-style: none;
  padding: 0;
  margin: 0;
}
```

사실 이 방법은 css의 display: grid를 사용하지 못하는 상황에서 만든 방법이다.

(크로스 브라우징 이슈 때문)

만약 grid 시스템을 사용할 수 있다면, 복잡하게 생각할 것 없이 grid로 화면을 구성하면 될 것이다.
(혹은 라이브러리를 쓰거나)