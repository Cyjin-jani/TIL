## JS Date 날짜간 차이 구하기
<br>

- 시작 날짜와 종료 날짜를 가지고 두 날짜 사이의 차이를 구하는 방법이다.

1. 일수 차이 구하기

```tsx
const getDays = (arr: Date[]) => {
    if (arr.length !== 2) {
      return 0;
    }
    const startDate = arr[0];
    const endDate = arr[1];

    const btm = endDate.getTime() - startDate.getTime();
    const btd = btm / (1000 * 60 * 60 * 24);
    return btd;
  };
```

1. 주차 차이 구하기
- 일수 차이에 * 7을 해주면 된다 (1주일이 7일이므로)

```tsx
const getWeekDiff = (arr: Date[]) => {
    if (arr.length !== 2) {
      return 0;
    }
    const startDate = arr[0];
    const endDate = arr[1];

    const dateDiff = endDate.getTime() - startDate.getTime();
    const result = dateDiff / (1000 * 60 * 60 * 24 * 7);
    return Math.floor(result);
  };
```

1. 월 차이 구하기
- 일수 차이에 * 30을 해주면 된다 (한 달이 보통 30일이므로)

```tsx
const getMontyDiff = (arr: Date[]) => {
    if (arr.length !== 2) {
      return 0;
    }
    const startDate = arr[0];
    const endDate = arr[1];

    const dateDiff = endDate.getTime() - startDate.getTime();
    const result = dateDiff / (1000 * 60 * 60 * 24 * 30);
    return Math.floor(result);
  };
```