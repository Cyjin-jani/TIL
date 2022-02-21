## a 태그로 파일 다운로드 구현
<br>

- href에 파일 위치 주소를 넣어준다.
- download 속성에는 파일명을 커스텀할 수 있다. (아무것도 넣지 않으면 그냥 본래 파일명으로 다운)

```tsx

const testUrl =
    'https://w3.org/WAI/ER/tests/xhtml/testfiles/resources/pdf/dummy.pdf'; 

<a href={testUrl} download="myTestFile" target="_blank">
  <label>파일 다운로드</label>
</a>
```