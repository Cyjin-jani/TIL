## strictMode 8진수 리터럴 오류 해결하기
<br>

![errorImg](https://user-images.githubusercontent.com/57857367/149153907-6ea7c795-7d94-42c7-a926-26d44c86272d.png)

회사에서 작업 후 빌드에러가 나길래 뭔가 하고 봤더니  위와 같은 에러가 발생했다...

직접 저 에러를 발생시킨 건 아니지만, 나였어도 충분히 놓치고 실수할만한 에러라고 생각되어 한 번 정리해본다.

js에서 strict-mode를 사용하게 되면 위에서 나온 것 처럼 8진수 리터럴을 사용할 수 없다.

8진수 구문은 모든 브라우저에서 앞에 0을 붙여서 지원된다.

```tsx
0644 === 420
"\045" === "%"
```

ECMAScript 2015에서는 접두사에 “0o”를 붙여서 8진수를 지원한다고 한다.

```tsx
var a = 0o10; // ES6: 8진수
```

가끔 앞에 붙은 0이 무의미하다는 생각에, 정렬용으로 아래와 가이 사용하는 경우가 있는데, 이는 숫자를 바꿔버릴 위험이 있다! 그렇기에 잘못 사용될 수 있는 부분을 막는 여지가 존재하므로, strict-mode에서는 아래와 같은 구문은 에러가 된다.

```tsx
"use strict";
var sum = 015 + // !!! 구문 에러
          197 +
          142;
```

물론 위의 경우와 다르게, 회사에서 사용되었던 부분은 theme에서 style을 정의하다가 발생한 것이었다.

```tsx
export const neutral = {
  000: '#FFF', // -> 이 구문이 strict-mode에서는 8진수를 지원하지 않아 에러다!
  100: '',
  200: '',
  300: '',
  400: '',
  500: '#AFB1B8',
  600: '',
  700: '#686E76',
  800: '',
  900: '#1A1F2B',
};
```

따라서, 굳이 0을 사용해야 한다면 아래와 같이 사용해야만 한다.

```tsx
export const neutral = {
  0: '#FFF', // -> 에러가 나지 않는다!
  100: '',
  200: '',
  300: '',
  400: '',
  500: '#AFB1B8',
  600: '',
  700: '#686E76',
  800: '',
  900: '#1A1F2B',
};
```

참고

[https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Strict_mode](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Strict_mode)