## class validator 폰 번호 관련 유효성 체크
<br>

input에 유효한 전화번호 형식을 입력하지 않으면, validation error message를 내도록 하여,
올바른 값을 입력받도록 구현해야 하였다.

class-validator를 사용하고 있었으나, 기존에는 @IsString() 이라는 형태만 사용하고,
프론트에서 form.watch를 걸거나 해서 유효한 번호 형식을 체크했었다.

작성해야하는 로직 코드도 많고, 제대로 동작하기까지 다소 시행착오를 거치는 불편함이 있었다 (그마저도 많은 변수들을 처리해야 하니 버그가 많았다.)

그래서 찾아본 결과 class-validator 패키지 자체에서 제공하는 IsPhoneNumber가 있었다.
지역을 설정하면 그에 맞도록 번호 형식을 체크해주는 편리한 데코레이터였다.

올바른 번호 형식 체크에는 하이푼(’-’)의 입력 여부를 신경쓰지 않고  번호 체크를 하였으나,
현 프로젝트에서는 하이푼이 제거된 채로 서버에 body request가 전송되어야 했다.
그래서 Transform(class-transformer 패키지 기능)을 이용하여 하이푼을 제거했다.

전체 코드는 다음과 같다.

```tsx
  @IsPhoneNumber('KR', { message: '올바른 번호 형태가 아닙니다.' })
  @Transform((v) => {
    return v.value.replace(/-/gi, '');
  })
  @IsNotEmpty()
  userPhoneNumber: string;
```