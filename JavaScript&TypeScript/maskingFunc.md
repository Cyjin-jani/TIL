## JS 카드번호, 계좌번호 마스킹 처리 함수 구현
<br>


1. 계좌번호 앞 3자리 빼고 마스킹

```tsx
export default function bankNumFormatter(bankNum: string) {
  let temp = '';
  const bankAccount = bankNum.trim();
  temp = bankAccount.substring(0, 3);

  for (let i = 3; i < bankAccount.length; i++) {
    temp = temp + '*';
  }

  return temp;
}
```

1. 카드 번호 16자리 기준 마스킹 (앞 4번호 빼고)

```tsx
export default function cardNumFormatter(cardNum: string) {
  let MaskingData = '';

  const cardNumFormat = cardNum
    .toString()
    .replace(/\B(?<!\.\d*)(?=(\d{4})+(?!\d))/g, '-');

  MaskingData = cardNumFormat.replace(
    /(\d{4})-(\d{4})-(\d{4})-(\d{4})/gi,
    '$1-****-****-****'
  );
  return MaskingData;
}
```