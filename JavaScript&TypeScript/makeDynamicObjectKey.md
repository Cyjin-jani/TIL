## JS 동적으로 Object key생성
<br>

js에서 코드를 작성하다 보니, form을 reset해주어야 하는 경우가 있었는데,

모든 폼을 리셋하면 안되고, 특정 name만을 reset해주어야 했다.

여기서 문제가, form 요소의 name이 정적인게 아니고, 동적으로 바뀌는 것이었기에,

```jsx
form.reset({
	typicalName: '',
});
```

위와 같은 식으로 할 수가 없다... (typicalName이 계속해서 바뀌기 때문...)

그래서 Object의 키와 값을 새롭게 동적으로 생성하는 로직이 필요해졌다.

```jsx
data.options.forEach((option) => {
	const selectId = option.id;
	const formSelect = {
	  [selectId]: undefined,
	};
	
	form.reset(formSelect);
});
```

위와 같이 세팅하자, 딱 필요한 form의 요소들만 undefined로 값이 초기화 되었다.