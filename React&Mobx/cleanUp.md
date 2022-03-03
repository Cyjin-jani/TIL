## 리액트 useEffect 메모리 cleanUp
<br>

리액트에서 useEffect를 사용하는 경우, 때때로 componentWillUnmount에 해당하는 라이프 사이클에서

메모리 누수를 막기 위한 cleanUp을 해주어야 하는 경우가 있다.

보통 Timeouts, subscriptions, event listeners 등의 경우가 그 대상이다.

cleanup을 하는 방법은 다음과 같다.

타임아웃의 경우

setTimeout을 했다면, useEffect내에서 return 시 clearTimeout을 해준다.

```tsx
import { useState, useEffect } from "react";
import ReactDOM from "react-dom";

function Timer() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    let timer = setTimeout(() => {
    setCount((count) => count + 1);
  }, 1000);

  return () => clearTimeout(timer)
  }, []);

  return <h1>I've rendered {count} times!</h1>;
}

ReactDOM.render(<Timer />, document.getElementById("root"));
```

subscription의 경우는 react-hook-form 라이브러리의 watch 메서드를 활용해보았다.

구독 - 구독 해제 예시

```tsx
useEffect(() => {
    const subscription = profileForm.watch((value) => {
      value.name !== vm.data.name || value.password.length > 0
        ? setVisibleProfileEditButton(true)
        : setVisibleProfileEditButton(false);
    });

    return () => subscription.unsubscribe();
  }, [profileForm.watch('name'), profileForm.watch('password')]);
```

이러한 처리를 해주면, 불필요한 메모리 누수를 막을 수 있다..