## Date객체 사용하기 - 특정 날짜의 요일 구하기
<br>

- Date의 getDay를 사용하면 된다.
- 0부터 6까지의 범위값을 가지고 있기에, 일요일부터 시작하는 요일 배열을 만든다. 
(아래 코드에서 week배열)
- getDay의 값을 week배열의 인덱스로 찾으면 원하는 요일이 나온다.

```tsx
function leftPad(value: number) {
  if (value >= 10) {
    return value;
  }
  return `0${value}`;
}

export function formattedDateAndDay(data: Date, delimiter = '.') {
  const year = data.getFullYear();
  const month = leftPad(data.getMonth() + 1);
  const day = leftPad(data.getDate());

  const week = ['일', '월', '화', '수', '목', '금', '토'];
  const dayOfWeek = week[data.getDay()]; 

  return `${[year, month, day].join(delimiter)}(${dayOfWeek})`;
}

// ex: 2022.02.14(월)
```