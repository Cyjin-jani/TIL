## styled-component를 쓴다면 꼭 필요한 babel-plugin-styled-components
<br>

현재 회사에서 프로젝트를 하는 경우, babel-plugin-styled-components를 사용한다... 

이전까지는 그 사용이유를 잘 몰랐으나 개발자도구 요소에서 classname에 컴포넌트 이름이 뜨게 하여 어떤 컴포넌트의 styled된 element인지 파악하기 쉽게 하기 위함이었다.

즉, 디버그에 매우 용이한 좋은 plugin이다!

세부 설정은 아래와 같다. (배포환경인 경우에는 name을 가릴 수 있다!)

```tsx
[
        'babel-plugin-styled-components',
        {
          ssr: true,
          displayName: isProd ? false : true,
          fileName: isProd ? false : true,
          pure: true,
          minify: isProd ? true : false,
          transpileTemplateLiterals: false,
        },
      ],
```

참고

[https://blog.woolta.com/categories/1/posts/198](https://blog.woolta.com/categories/1/posts/198)