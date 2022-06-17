## 범용적으로 withComma 사용하기
<br>

보통 우리는 가격을 표시할 때 콤마(,)를 3자리 단위마다 표시하여 사용한다.

이 3자리 마다 콤마를 넣어 표시하는 함수로 withCommas를 만든 적이 있다. (대충 아래와 같은 식이다.)

```jsx
export default function withCommas(target?: number | string) {
  if (target === null || target === undefined) {
    return;
  }

  return target.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ',');
}
```

withCommas를 기존에는 함수로 만든 뒤, export 하여
필요한 곳에서 해당 함수를 import 하여 사용하는 방식으로 사용하였다.

그러나 이 방법은 함수로 사용되기에, 항상 parameter인자를 넣어주어야 하는 번거로움이 있었다.

비교적 쉽게 사용하는 방법이자, 자바스크립트여서 사용이 가능한 새로운 방법이 있었다.

바로, prototype에 함수 자체를 추가해 버리는 방법이다.

구현 방식은 아래와 같다.

```jsx
Number.prototype.withComma = function () {
  if (this === 0) {
    return 0;
  }

  return this.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ',');
};
```

이렇게 하면, Number 타입인 변수는 전부 withComma()로 손쉽게 콤마를 사용할 수 있게 된다.

사용 방법은 아래와 같다.

```jsx
// import 해준다.
import '@helper/withCommas';

// 중략...
return (
	// 중략...
	<MileageLabel>{ml.point.withComma()}</MileageLabel>
);

```