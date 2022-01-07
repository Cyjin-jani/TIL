## nextJs에서 useCallback을 활용한 클립보드 복사하기
<br>

클립보드 복사를 위해 document.execCommand('copy')를 활용하였다.

next js 에서는 useRef를 쓰기 까다로워서 (null, undefined로 읽히다보니..)

useRef대신 useCallback을 활용하였다.

state로서 HTMLInputElement 노드를 저장할 것 하나와, full url path를 저장할 state.

총 두개를 사용함.

```jsx
const [fullUrl, setFullUrl] = useState('');
const [refNode, setRefNode] = useState<HTMLInputElement>();
```

일단, FULL URL Path를 얻기 위해, next/router의 useRouter를 이용하여 경로를 얻고,

window객체에서 host를 얻었다. 

(단, next.config.js  설정파일에서  basepath 설정하면 window객체에서 안받아와도 됨)

```jsx
// URL 클립보드 복사를 위한 fullpath 얻기
  if (typeof window !== undefined) {
    const host = window.location.host;
    const url = router.asPath;
    const fullURL = host + url;
    setFullUrl(fullURL);
  }
```

(basePath 설정 예시)

```jsx
module.exports = {
  basePath: '/docs',
}
```

그리고 useRef대신 callback을 활용하여 dom을 지정할 수 있게 하였다.

```jsx
// URL 주소가 담긴 textArea 가져오기
const urlRef = useCallback((node) => {
  if (node) {
    setRefNode(node);
  }
}, []);
```

원하는 dom element에 ref에다가 넣어주면 끗.

물론 url 주소 공유 버튼만 클릭하면 작동하는 것이니, render 영역에는 textarea를 보이지 않게 만들어 준다.

```jsx
{/* 클립보드 저장을 위한 textarea */}
<textarea ref={urlRef} value={fullUrl} readOnly style={{ width: 0; height: 0; opacity: 0;}} />
```

url주소 버튼 클릭 시 실행되는 이벤트 (클립보드 저장)

```jsx
const handleShareURL = () => {
  if (!document.queryCommandSupported('copy')) {
    return enqueueSnackbar('복사 기능을 지원하지 않는 브라우저입니다', {
      variant: 'error',
    });
  }
  // 클립보드에 저장
  if (refNode) {
    refNode.select();
    document.execCommand('copy');
    enqueueSnackbar('클립보드에 복사되었습니다', {
      variant: 'success',
    });
  }
};
```

참고 사이트

[https://ratelmorning.tistory.com/41](https://ratelmorning.tistory.com/41)