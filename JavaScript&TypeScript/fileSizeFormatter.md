## file size formatting 함수
<br>

파일 업로드 시 파일의 이름과 크기를 보여줘야 하는 경우가 생겼다.

파일은 일반적으로 js에서는 바이트 크기 단위이기에, kb나 mb 등으로 포맷팅이 필요하였다.

다음과 같이 구현하였다.

```tsx
 function formatFileSize(
  bytes: number,
  decimalPoint?: number,
  notCapital?: boolean
) {
  if (bytes == 0) return '0 Bytes';
  const k = 1000,
    dm = decimalPoint === 0 ? 0 : decimalPoint || 2,
    sizes = notCapital
      ? ['bytes', 'kb', 'mb', 'gb', 'tb', 'pb', 'eb', 'zb', 'yb']
      : ['Bytes', 'KB', 'MB', 'GB', 'TB', 'PB', 'EB', 'ZB', 'YB'],
    i = Math.floor(Math.log(bytes) / Math.log(k));
  return parseFloat((bytes / Math.pow(k, i)).toFixed(dm)) + sizes[i];
}
```