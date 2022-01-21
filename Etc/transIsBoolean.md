## classValidator의 boolean값 validation
<br>
classValidator에는 IsBoolean()이라는 데코레이터가 있다.

boolean 값인지 유무를 판별해 주는 데코레이터인데,

query를 통해 dto의 필드를 주고받으면 string형태가 되어, boolean을 보냈음에도 string을 받아버리니

validation 에러가 발생한다.

이를 회피하기 위한 방법으로 class-transformer에서 Transform을 활용하여 boolean으로 값을 만들어 줄 수 있다.

코드 예시는 아래와 같다.

```tsx

import PaginationQueryDto from '@shared/domain/dto/PaginationQueryDto';
import { IsBoolean, IsOptional } from 'class-validator';
import { Transform } from 'unsafe-class-transformer'; //class-transformer를 커스터마이징 함

export default class FindDto extends PaginationQueryDto {
  @IsBoolean()
  @Transform(({ value }) => {
    if (value === undefined) {
      return undefined;
    }
    if (typeof value === 'string') {
      return value === 'true';
    }
    return value === true;
  })
  @IsOptional()
  isRead?: boolean;
}
```