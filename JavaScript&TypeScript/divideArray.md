## array 특정 갯수로 배열 나누기
<br>

하나의 객체 리스트를 두 배열로 나누어 렌더링을 해주어야 하는 경우가 있었다.

예를 들어 [a, b, c, d, e, f, g, h] 라는 배열이 있다면,

```
a, b   |  e, f
c, d   |  g, h
```
이러한 형태로 배열의 데이터를 렌더링해줘야 했었다.

그래서 화면을 두 개로 나누어서 각각 양쪽에 map을 돌려서 배열을 뿌려주는 것으로 하려고 하다보니, 기존의 1차원 배열을 두 개의 배열로 나누어야만 했다. 단순 나눈다기 보단, 2차원 배열로 만들어야했다.

이처럼, 특정 갯수로 배열을 나누는 방법을 고민했고,
아래와 같은 방법들로 구현해볼 수 있었다.

방법1

```css
function chunk(arr: any[], size: number) {
  const j = arr.length;
  const temparray = [];
  const chunk = size;
  for (let i = 0; i < j; i += chunk) {
    temparray.push(arr.slice(i, i + chunk));
  }
  return temparray;
}
```


방법2

```css
function division(arr: any[], n: number) {
  const len = arr.length;
  const cnt = Math.floor(len / n) + (Math.floor(len % n) > 0 ? 1 : 0);
  const temp = [];

  for (let i = 0; i < cnt; i++) {
    temp.push(arr.splice(0, n));
  }
  return temp;
}
```

