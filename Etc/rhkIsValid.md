## react hook form 다루기 - isValid
<br>

formstate의 isValid를 활용하면, form 데이터 작성시 유효성 검사를 통과하여 문제가 없는 경우에 true를 반환하게 된다. 이는 주로 form 전송 버튼을 초기에 disabled 처리해 두었다가 사용자가 필수 입력 필드를 모두 입력했을 경우, button click액션이 가능하도록 처리하는 기능에서 쓰인다.

사용 방법은 다음과 같다.

일단, react-hook-form에서 유효성 검사하는 방식은 여러 가지가 있는데,

class-validator를 이용하여 검사해본다.

resolver를 useForm에 등록을 제대로 해주는 것이 필요하다. 그렇지 않으면 예상대로 동작하지 않게 된다.

```tsx
const form = useForm<CreateDto>({
    mode: 'all',
    resolver: classValidatorResolver(CreateDto),
  });
```

또한, mode가 중요한데, onchange나 onBlur, onSubmit등 다양한 타이밍에 validation체크를 해줄 수 있다.

일단 기본적으로 값을 적용하지 않는다면 mode는 onSubmit으로 되어있어서, form전송 버튼을 클릭하는 경우에만 유효성 검사가 동작한다.

isValid는 보통 아래와 같이 사용한다.

```tsx
const isValid = form.formState.isValid; // 폼 전송 버튼 활성화 여부 판단

// (...중략)

<Button
  type="submit"
  label="전송버튼"
  disabled={!isValid}
/>
```

formState에서 form 내부 요소들의 변화가 일어나는 경우, 감지해서 유효성 검사를 통과했는지에 대한 여부를 알려준다. 물론 위에서 mode를 어떻게 설정하느냐에 따라 form 내부 값을 변경해도 유효성검사가 일어나지 않을 수 있다.

번외 (CreateDto 간단 예시)

 - name과 email이 정말로 있는지 확인, string인지 확인하는 유효성 검사를 가지고 있다.

```tsx
export default class CreateDto {
	@IsString()
  @IsNotEmpty()
  name: string;

	@IsString()
  @IsNotEmpty()
  email: string;
}
```

참고

[https://react-hook-form.com/api/useform](https://react-hook-form.com/api/useform)

[https://react-hook-form.com/api/useform/formstate](https://react-hook-form.com/api/useform/formstate)