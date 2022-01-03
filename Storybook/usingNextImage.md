## storybook에서 next/image 사용하기
***
<br>
storybook에서는 next/image에서 제공하는 최적화가 제대로 작동하지 않는다. (심지어 에러를 뱉어낸다 ㄷ) <br>

공식문서를 보면, storybook은 넥스트js와 통합되어 돌아가는게 아니므로, 두 가지 처리를 해주어야 한다고 한다.
<br>

> Because Storybook runs these components in isolation of Next.js framework-integrations, we need to configure it in two important ways:
> 1. Serve the Next.js `public` directory in Storybook
> 2. Add the `unoptimized` prop to Next.js Image component in all stories

아래와 같이 package.json을 설정해주고,

```tsx
// package.json

"scripts": {
-    "storybook": "start-storybook -p 6006",
-    "build-storybook": "build-storybook"
+    "storybook": "start-storybook -p 6006 -s ./public",
+    "build-storybook": "build-storybook -s public"
}
```

스토리북 preview.js에 아래 코드를 넣어준다.

```tsx
// .storybook/preview.js
import * as NextImage from "next/image";

const OriginalNextImage = NextImage.default;

Object.defineProperty(NextImage, "default", {
  configurable: true,
  value: (props) => <OriginalNextImage {...props} unoptimized />,
});
```

위 과정을 거치면 storybook에서 next/image를 사용할 수 있게 된다!
<br>
<br>
참고 <br>
[공식문서](https://storybook.js.org/blog/get-started-with-storybook-and-next-js/)