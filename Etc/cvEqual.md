## class validator boolean 값 판별 
<br>

isBoolean을 쓰면 그냥 값이 있으면 true가 되서 패스 해 버린다.

값 이 무조건 true여야 하는 boolean 필드가 있다면 (예: 동의하기 체크박스 등)

isBoolean보다는 Equal을 사용하는 것이 좋다.

사용 예시

```tsx
  @Exclude({ toPlainOnly: true })
  @Equals(true)
  @IsNotEmpty()
  policyAgreement: boolean;
```

번외

Exclude는 변환 시 해당 프로퍼티를 스킵하도록 한다. 

옵션에는 toPlainOnly와 toClassOnly가 있는데, 어떤 경우에 스킵을 할 지 정하는 것이다.

보통 plain에서는 말 그대로 plainObject이므로, 보통 dto에서 사용되고는 한다.