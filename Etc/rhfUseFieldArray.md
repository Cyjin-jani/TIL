## react hook form useFieldArray 사용시 주의점
<br>

useFieldArray 사용할 때, append를 하는 경우, 자동으로 input 같은 폼 요소에 포커싱을 해준다.

이게 보통의 경우 편리하지만, 필요에 따라 불편한 경우가 존재했다.

focus를 없애는 방법을 useEffect와 ref current.focus()를 활용해보거나, react-hook-form의 setFocus 함수를 활용해보았지만,,,,

아무것도 먹히지 않았다...

찾다가 겨우 발견한 방법은 바로 append시 focus관련 옵션을 줄 수 있다는 것!!!

아래와 같이 사용이 가능하다.

```tsx
const { fields, append, remove } = useFieldArray({
  control,
  name: 'replies',
});

append(
  {
		author: '',
    content: '',
  },
  { shouldFocus: false } // 자동으로 포커싱 되지 않도록 한다.
);
```

공식문서

[https://react-hook-form.com/api/usefieldarray](https://react-hook-form.com/api/usefieldarray)