## 타입 검사하기
<br>
타입스크립트를 사용하면서, 특정 타입 별로 다른 코드가 동작되도록 하는 경우가 많이 있다.

단순한 예를 들면, button 컴포넌트를 만들 때, 버튼의 라벨을 string으로만 한정지어 받을 수도 있지만,

아이콘이 들어가거나 단순 글자가 아닌 경우에는, string 대신 다른 element를 받을 수 있어야 한다.

그래서 보통 label에 아래와 같이 타입을 정의하곤 한다.

```tsx
type ButtonProps = {
	label: string | JSX.Element
	// ...rest
}
```

타입 정의를 한 뒤, string인 경우에는 Typography (혹은 label 태그 등)으로 감싸준다.

하지만, label Jsx.Element인 경우에는 그냥 라벨을 그리도록 해야하므로 이에 따른 타입 검사 (분기처리)가 필요하다. 이럴 때 활용되는 것이 typeof 키워드이다.

typeof에서 확인할 수 있는 원시타입들은 다음과 같다.

: string, boolean, undefined, number, symbol

검사는 아래와 같이 한다.

예1) - 일반적인 js에서

```tsx
if (typeof label === 'string') {
	// string인 경우 처리하기
}
```

예2) - jsx에서

```tsx
{typeof label === 'string' ? (
  <LabelTypo>{label}</LabelTypo>
) : (
  <>{label}</>
)}
```

하지만, 만약 해당 타입이 primitive한 경우가 아닌 특정 객체, class 등의 따로 정의된 타입인 경우엔 어떻게 할까?

답은 instanceof 키워드를 사용하는 것이다.

```tsx
type SomeType = {
	// something
}
if ( data instanceof SomeType) {
	// ...do something
}
```

예시 코드

```tsx
const uploadHandler = (data: File | FileList) => {
  if (data instanceof File) { // instanceof를 사용하여 타입 확인
	// data의 타입이 File인 경우 처리하는 부분
    setImg([...img, URL.createObjectURL(data)]);
  } else {
    const fileList = Object.values(data);
    const imgList = fileList.map((file) => {
      return URL.createObjectURL(file);
    });
    setImg([...img, ...imgList]);
  }
};
```