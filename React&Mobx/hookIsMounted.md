## isMounted를 hook으로 만들어 사용하기
<br>

기존에는 컴포넌트에서 컴포넌트가 마운트 된 상태인지를 파악하기 위해 isMounted 라는 변수를 state를 이용하여 관리하였었다.

예시 코드)

```tsx
const [isMounted, setIsMounted] = useState(false);

useEffect(() => {
 if (!isMounted) {
	setIsMounted(true);
 } else {
	// do something when component is mounted
 }
}, [isMounted])
```

하지만 더 간결하게 hook으로 만들어 사용하는 방법이 있다.

```tsx
// hook
import { useCallback, useEffect, useRef } from 'react';

function useIsMounted() {
  const isMounted = useRef(false);

  useEffect(() => {
    isMounted.current = true;

    return () => {
      isMounted.current = false;
    };
  }, []);

  return useCallback(() => isMounted.current, []);
}

export default useIsMounted;
```

사용법은 다음과 같다.

```tsx
import { FC, useState, useEffect } from 'react';

import useIsMounted from '../../hooks/useIsMounted'

type Props = {}

const Component: FC<Props> = ({}) => {

  const delay = (ms: number) => new Promise(resolve => setTimeout(resolve, ms));

  const [data, setData] = useState('loading');
  const isMounted = useIsMounted();

  // simulate an api call and update state
  useEffect(() => {
    if (isMounted()) {
    void delay(1000).then(() => {
        setData('mounted');
      });
    } else {
      console.log('not mounted yet');
    }
  }, [isMounted]);

    return <div>{data}</div>;
}
```