## Date객체 사용하기 - 한달 전 날짜 구하기
<br>

Date를 한달 전 날짜를 구하는 방법이다.
크게 new Date()로 감싸주지 않으면, 날짜 객체가 아니게 된다.

계산 된 날짜가 number타입으로 받아지므로, 마지막에 감싸주어 Date타입으로 만들어 주었다.

```tsx
const nowDate = new Date(); // 오늘 날짜

// 오늘 기준 한달 전 날짜
const monthAgo = new Date(new Date().setMonth(new Date().getMonth() - 1)); 
```