## 스크롤바 숨기기
<br>

스크롤이 되어야 하면서 동시에, 스크롤 바가 보이지 않도록 해야하는 경우가 있을 수 있다.

그러한 경우에는 -webkit-scrollbar를 이용하면 된다. (크롬 기준)

이외에 ms, firefox 등 별도 브라우저의 경우 아래처럼 별도 세팅을 해주면 된다.

```jsx
const ChildrenWrapper = styled.div`
  height: 100%;
  overflow-y: scroll;
  -ms-overflow-style: none; /* IE and Edge */
  scrollbar-width: none; /* Firefox */
  ::-webkit-scrollbar {
    display: none; /* Chrome, Safari, Opera*/
  }
`;
```