## next js .env 환경변수 설정 및 불러오기
<br>

next js 에서 next.config.js를 정의해서 사용하고 있다면, env파일 설정 후 불러올 때는 바로 process.env.XXXX가 아니라 next/config의 getConfig로 불러와야 한다.

사용 예시는 다음과 같다.

```tsx
import getConfig from 'next/config';
const { publicRuntimeConfig } = getConfig();
```

환경변수 설정방식

env파일에 아래와 같이 설정하고 나면,

```js
// .env
MODE=DEVELOPMENT
CLIENT_VERSION=1.0.0
CLIENT_HOST=LOCALHOST
API_HOST=http://localhost:3001/api
API_VERSION=v1
```

아래와 같은 식으로... next.config.js에다가 해당 환경변수를 설정해준다. 

```tsx
publicRuntimeConfig: {
    mode: process.env.MODE,
    clientVersion: process.env.CLIENT_VERSION,
    clientHost: process.env.CLIENT_HOST,

    apiHost: process.env.API_HOST,
    apiVersion: process.env.API_VERSION,

  },
```

위와 같이 설정을 하면 맨 위의 next/config의 getConfig에서 publicRuntimeConfig를 가져왔기에

해당 환경변수의 사용이 가능해진다.
