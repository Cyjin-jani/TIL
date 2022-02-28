## JS 전화번호 포맷팅
<br>

전화번호를 string으로 받아서 일정한 번호 형식으로 포맷을 변환해주는 함수이다.


이러한 함수를 내부에서 구현해주는 라이브러리들도 있지만,

한 번 연습 겸 해당 로직을 구현해보았다. 

```tsx
export default function phoneFormatter(num: string, hide?: boolean) {
  let formatNum = '';

  if (num.length == 11) {
    if (hide) {
      formatNum = num.replace(/(\d{3})(\d{4})(\d{4})/, '$1-****-$3');
    } else {
      formatNum = num.replace(/(\d{3})(\d{4})(\d{4})/, '$1-$2-$3');
    }
  } else if (num.length == 8) {
    formatNum = num.replace(/(\d{4})(\d{4})/, '$1-$2');
  } else {
    if (num.indexOf('02') == 0) {
      if (hide) {
        formatNum = num.replace(/(\d{2})(\d{4})(\d{4})/, '$1-****-$3');
      } else {
        formatNum = num.replace(/(\d{2})(\d{4})(\d{4})/, '$1-$2-$3');
      }
    } else {
      if (hide) {
        formatNum = num.replace(/(\d{3})(\d{3})(\d{4})/, '$1-***-$3');
      } else {
        formatNum = num.replace(/(\d{3})(\d{3})(\d{4})/, '$1-$2-$3');
      }
    }
  }

  return formatNum;
}
```