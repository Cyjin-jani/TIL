## next node modules import 관련 에러 핸들링
<br>

ios 스크롤 관련 문제로, 'seamless-scroll-polyfill' 라는 외부 패키지 하나를 설치하고, 실행을 했는데 아래와 같은 메세지가 발생했다....
![Untitled (2)](https://user-images.githubusercontent.com/57857367/151989699-c4f9904e-83c1-4ac4-b126-8817d9a52712.png)

js 모듈에 관한 임포트 형식이 맞지 않는 듯 하다. import 방식과 require 방식의 혼합?된 사용이 안되기 때문에 해당 부분을 바꾸어줄 필요가 있어 보였다.


조금 조사를 해보니, next js에서는 아직 commonJs 방식이라, require을 사용한 모듈 관리를 하지만,

해당 외부 패키지에서는 ES6 imports 방식을 사용하여 충돌이 난 듯 하다.

이를 해결하기 위해 새로운 패키지 하나를 설치하여 해결해주도록 하자

transpile을 해주는 패키지: https://github.com/martpie/next-transpile-modules

사용법은 다음과 같다.

```tsx
const withTM = require('next-transpile-modules')([
  'react-hook-form',
  'seamless-scroll-polyfill',
]);
```

게다가, 조금 조사하다 보니 외부 라이브러리(패키지)가 IE를 지원하지 않는 경우에, transpile을 해주지 않으면 크로스 브라우징 이슈 또한 발생하는 듯 하다!! 

next js가 기본적으로 외부 패키지를 tranpile해주지 않다 보니 babel이 자동으로 처리해 주지 못 한다.

그렇기에 해당 이슈가 있다면 역시 transpile을 해주어야 한다.

만약 사용하는 플러그인이 많다면, 아래와 같이 플러그인 패키지를 이용하여 한 번에 처리가 가능하다

```tsx
const withPlugins = require("next-compose-plugins");
const withTM = require("next-transpile-modules")([
  "some-module",
  "and-another",
]);
module.exports = withPlugins([withTM], {
  // ...
});
```

