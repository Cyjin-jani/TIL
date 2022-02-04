## JS 시간표시 계산 함수
<br>

보통 알림 같은 정보를 표시하는 경우, 해당 알림 정보가 생성된 시간이 현재 시각에서 얼마나 전에 생성되었는지를

표시해주는 기능이 필요한 경우가 있다.

물론 그냥 바로 0000년00월00일 00시00분  이런식으로 표현하는 경우도 있지만,

좀 더 세세한 단위로 나누어서 몇 초 전, 몇 분 전 몇 시간 전 등의 표시방식을 보이는 경우에는 해당 시간을 계속해서 계산해주어야 한다.

이를 위해 함수를 하나 만들어 보았다.

물론, 프로젝트 요구사항 별로 표시방식이 얼마든지 달라질 수 있다.

자세한 코드는 다음과 같다.

```tsx
// 요구사항에 따라 n일전, n주전 등을 표시할 수 있습니다.
export default function displayTimes(target: Date) {
  if (target === null || target === undefined) {
    return;
  }
  const currentDate = new Date();
  const milliSeconds = currentDate.getTime() - target.getTime();
  const seconds = milliSeconds / 1000;
  if (seconds < 30) return `30초 전`;
  // if (seconds < 60) return `방금 전`;
  const minutes = seconds / 60;
  if (minutes < 60) return `${Math.floor(minutes)}분 전`;
  const hours = minutes / 60;
  if (hours < 24) return `${Math.floor(hours)}시간 전`;
  return formattedDateAndTime(target);
  // const days = hours / 24;
  // if (days < 7) return `${Math.floor(days)}일 전`;
  // const weeks = days / 7;
  // if (weeks < 5) return `${Math.floor(weeks)}주 전`;
  // const months = days / 30;
  // if (months < 12) return `${Math.floor(months)}개월 전`;
  // const years = days / 365;
  // return `${Math.floor(years)}년 전`;
}

// 날짜 포멧팅 함수
export function formattedDateAndTime(data: Date, delimiter = '.') {
  const year = data.getFullYear();
  const month = leftPad(data.getMonth() + 1);
  const day = leftPad(data.getDate());
  const hour = leftPad(data.getHours());
  const minute = leftPad(data.getMinutes());
  return `${[year, month, day].join(delimiter)} ${hour}:${minute}`;
}
```