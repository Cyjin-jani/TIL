## 재귀함수 기초
<br>

1. 1부터 100까지의 합

```js
  function sum(number) {
    if (number === 1) {
      return number;
    }
    return number + sum(number-1);
  }

  console.log(sum(100));
```

2. 2진수 변환

```js
  function convert(number) {
    // ===으로 비교하지 않은 이유는 string의 형태로 붙여줘야하기 떄문 (1, 0이 연산이 되면 안됨)
    if (number == 1|| number == 0) {
      return String(number);
    }
    return convert(Math.floor(number / 2)) + String(number % 2);
  }  

  console.log(convert(11));
```

3. 문자열 뒤집기
단순히 string을 array로 바꾸고 reverse를 할 수도 있지만, 재귀로 구현해 보았다.

```js
  function flipStr(str) {
    if (str.length === 1) {
      return str;
    }
    // 맨 마지막 글자를 먼저 적고, 맨 마지막 글자를 제외한 문자열을 다시 재귀.
    return str[str.length - 1] + flipStr(str.slice(0, str.length - 1));
  }  
  console.log(flipStr('abcde'));
```

4. 피보나치 순열 (1+1+2+3+5+8...)

```js
  function fibo(num) {
    if (num == 1 || num == 2) {
      return 1;
    }
    return fibo(num - 1) + fibo(num -2);
  }  
  console.log(fibo(6));
```


