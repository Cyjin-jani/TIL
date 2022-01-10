## ts 모듈 import에 대해..
<br>

npm 외부 패키지를 설치하고 사용하려는 도중, import 구문에서 에러가 발생했다....

```tsx
이 모듈은 'export ='를 사용하여 선언되었으며 'esModuleInterop' 플래그를 사용하는 경우에만 기본 가져오기와 함께 사용할 수 있습니다.
```

react 와 같이 export default가 아닌, export = Component 라던가 export as namespace Component 와 같은 경우에는, import Component from ‘component’  혹은 import React from ‘react’ 로 사용할 수 없다.

이유:

default export가 없으니 어떤 것을 기본으로 받아와야 하는 지 모르기 때문에 오류가 뜨는 것이다.

외부 패키지를 보니... 다음과 같이 export를 하고 있다..

```tsx
export as namespace objectFillImages;

export = objectFillImages;

declare function objectFillImages(
    images?: string | HTMLElement | HTMLElement[] | NodeList | null,
    options?: { watchMQ?: boolean | undefined; skipTest?: boolean | undefined },
): void;
```

잘못된 사용법

```tsx
import objectFitImages from 'object-fit-images';
```

그래서 사용하려면, 아래와 같이 사용해야 한다.

```tsx
import objectFitImages = require('object-fit-images');
```

commonJs모듈을 받아오는 키워드인 require를 사용해줘야 에러가 발생하지 않는다. Es6 문법의 형태로 import 구문을 쓰면 에러가 나는 것이다.

그럼 사용이 절대 안되는건가..? 아니다!

tsConfig의 설정을 해주면 가능하다.

esModuleInterop 플래그를 true로 사용해주면, 기본 가져오기를 사용할 수 있게 된다.

저 플래그를 true로 해주면, 아래와 같이 바꿔주게 되는 것이다.

```tsx
const objectFitImages = __importDefault(require('object-fit-images'));
```

그래서 아래 예시처럼 compilerOption에 위 플래그 설정을 추가해주면 된다.

```tsx
// tsconfig.json

{
...
	"compilerOptions": {
	...
	"esModuleInterop" : true,
	}
}
```