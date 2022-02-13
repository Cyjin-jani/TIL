## 브라우저 윈도우 가로폭 가져오는 hook (for next js)
<br>

component 마운트 될 때, window의 가로폭을 가져올 수 있는 hook이다.

또한, window의 사이즈가 변경될 때 마다 가로폭의 변경된 사이즈를 가져올 수 있도록 resize 이벤트 리스너를 달아주었다.

```jsx
const [width, setWidth] = useState(0);

  useEffect(() => {
    setWidth(window.innerWidth);

    function handleWindowSizeChange() {
      setWidth(window.innerWidth);
    }

    window.addEventListener('resize', handleWindowSizeChange);
    return () => {
      window.removeEventListener('resize', handleWindowSizeChange);
    };
  }, []);
```


주로 이미지 비율 정해서 캐로젤 같은 데에서 해당 width에 맞는 비율의 height를 세팅하는 등의 경우에 사용이 가능하다.


보통의 CSR(Client-Side-Rendering)인 리액트 같은 경우는 사실 
코드에서 즉시 바로 window에 접근이 가능해서 window가 undefined일 경우를 고려하지 않을 수 있다.

그러나, next Js와 같은 SSR(Server-Side-Rendering)의 경우에는 초기에 코드를 컴파일하는 과정에서 window의 객체가 무엇인지 알지 못한다. (서버가 아닌, 브라우저 단에서 적용되기 때문)

따라서, state와 useEffect를 통해 window 객체가 undefined가 아님을 판별해서 가로폭 사이즈를 가져오는 로직이 필요한 경우가 있다.

```jsx
    // useEffect 내부 
    if (window) {
      // do something...
    }
```
