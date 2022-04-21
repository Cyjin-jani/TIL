## react-hook-form의 비제어 input에서 value 커스텀 세팅하기
<br>

보통의 경우, react에서 input을 만들게 되면 value, onChange를 사용한다.

그러나, react-hook-form을 사용하게 되면,

input을 controller로 감싸서 제어 컴포넌트로 만들지 않고,

register만 등록하여 비제어 컴포넌트로 만들어 사용하는 경우가 대부분이다.

이러한 경우에는 onChange로 value를 세팅하는게 아니어서, 만약에 공백을 제거해야 하는 상황에서는

그동안 classTransformer의 @TransForm()을 사용해서 변환해주거나 했다.

그러나 input의 register에 value를 세팅해주는 옵션이 존재했다.

바로 setValueAs이다.

사용법은 아래와 같다.

```tsx
<Input
  {...register(name, {
    setValueAs: (v) => {
      if (v && hasNoWhiteSpace) {
        return v.replace(/\s/gi, '');
      }
      return v;
    },
  })}
  error={error}
  disabled={disabled}
  {...rest}
/>
```

출처

[https://github.com/react-hook-form/react-hook-form/issues/1650](https://github.com/react-hook-form/react-hook-form/issues/1650)