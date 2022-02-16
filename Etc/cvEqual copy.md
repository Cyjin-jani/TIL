## class validator 선택적 유효성 검사 ValidateIf 
<br>

가끔, 필드가 필요에 따라 필수값이 되어야 하는 경우가 있다.

예를 들면, 회사 이름이 없다면 브랜드 이름이 있어야 하거나 브랜드 이름이 없으면 회사 이름이 있어야 하는 경우다.

이러한 경우 선택적으로 유효성 체크를 하기 위해서 ValidateIf라는 것을 사용한다.

사용법은 다음과 같다.

```tsx
import { ValidateIf, IsNotEmpty } from 'class-validator';

export class Post {
  otherProperty: string;

  @ValidateIf(o => o.otherProperty === 'value')
  @IsNotEmpty()
  example: string;
}
```

위와 같은 경우는, 서로 상관관계에 있다기 보다는 otherProperty의 값의 영향에 따라 example이 바뀌는 것이고 그 반대는 해당하지 않는다.

만약, 맨 위에서 예를 들었던, 회사 이름 ↔ 브랜드 이름 둘 중 하나는 무조건 필수적으로 있어야 하는 경우에는 어떻게 처리할까?

아래와 같이 처리할 수 있다.

```tsx
	@ValidateIf((o) => !o.brandName || o.companyName)
  @IsString()
  @IsNotEmpty()
  companyName: string;

  @ValidateIf((o) => !o.companyName || o.brandName)
  @IsString()
  @IsNotEmpty()
  brandName: string;
```

위처럼 처리하면, companyName의 유효성을 체크할 때, brandNamedㅣ 없거나 companyName이 있는 경우 유효성 체크를 진행한다.

마찬가지로, brandName의 유효성을 체크하는 경우에는 companyName이 없거나 brandName이 있는 경우 체크한다.

결국, 선택적으로 필드의 유효성 체크를 하는 방법은  @validateIf를 활용하면 된다.