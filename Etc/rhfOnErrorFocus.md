## react-hook-form 에러 시 포커싱 막기
<br>
react-hook-form을 쓰는 경우, 특히 유효성 검사를 위해 resolver를 쓴다면,

입력 폼에 에러가 있을 시 자동으로 focus가 해당 element로 간다. (uncontrolled form 요소인 경우에만)

만약, 에러 메세지를 alert로 띄워야 하는 경우에는 auto focus가 모바일에서 상당히 방해를 받는다.

focus → keyboard올라옴 → alret메세지 창 뜸 (키보드에 가려서 잘려서 보임) → alert창 닫은 후에 다시 focus됨

이러한 버그가 있기에, 이를 해제하는 방법을 찾아보니, 자동으로 focus가 되지 않도록 하는 옵션이 있었다.

~~(역시 rhf이야 믿고 있었다구!)~~

방법은 다음과 같다.

```jsx
const form = useForm({
    resolver: yupResolver(schema),
    shouldFocusError: false,
  });
```

위에서 처럼, shouldFocusError 옵션에 false를 주면 된다. (default는 true)

해당 옵션을 false로 준뒤, useEffect를 이용하여 에러가 있을 때 마다 alert를 띄워주었다.

```jsx
useEffect(() => {
    const errors = form.formState.errors;
    if (errors.companyName) {
      alert(errors.companyName.message);
      form.setFocus('companyName');
      return;
    }
    if (errors.phoneNumber) {
      alert(errors.phoneNumber.message);
      form.setFocus('phoneNumber');
      return;
    }
    if (errors.emailAddress) {
      alert(errors.emailAddress.message);
      form.setFocus('emailAddress');
      return;
    }
    if (errors.content) {
      alert(errors.content.message);
      form.setFocus('content');
      return;
    }
    if (errors.agreement) {
      alert(errors.agreement.message);
      return;
    }
  }, [form.formState.errors]);
```

참고

[https://react-hook-form.com/faqs](https://react-hook-form.com/faqs)