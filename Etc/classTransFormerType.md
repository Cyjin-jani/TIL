## class transformer @Type사용하기
<br>

class transformer에서 type 데코레이터의 사용성에 대해 알아볼 필요가 있다.

보통 쿼리를 통해 페이지네이션의 기능을 구현하는데, 한 가지 문제가 있었다. query는 string이라는 점. 

때문에 query.limit이 string이지만, class(dto)에서 number타입으로 사용되고 있는 경우, 처음에는 일일히 string을 number로 치환해주는 작업(parseInt 등)을 해주었었다.

그러나, class-transformer에서는 type이라는 데코레이터 메서드가 존재한다. 

타입을 변환해줄 때, type 데코레이터를 쓰면 편리하다. (단, 해당 타입으로 변환할 수 있는 값이어야 할 것이다. 예를 들어, 배열을 number로 바꾸는 등의 행위는 일어날 수 없다.)

```tsx
@Type(() => Number)
limit = 20;
```

공식문서

[https://www.npmjs.com/package/class-transformer#providing-more-than-one-type-option](https://www.npmjs.com/package/class-transformer#providing-more-than-one-type-option)

관련 질의응답

[class-transformer github issue](https://github.com/typestack/class-transformer/issues/179)