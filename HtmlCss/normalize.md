## css의 normalize를 위한 polished 패키지 사용
<br>

보통 css의 경우, reset이나 normalize를 통해 기존의 css를 정비? 하고 시작한다.

우리 회사에서는 normalize를 위해 polished라는 패키지를 사용한다.

[https://polished.js.org/docs/#installation](https://polished.js.org/docs/#installation)

사용법은 아래와 같다.

```tsx
import { normalize } from 'polished';
import { createGlobalStyle } from 'styled-components';

const Component = createGlobalStyle`
   ${normalize()};

   * {
     box-sizing: border-box;
   }

   html {
     height: 100%;
     line-height: 1;
   }

   #__next, body {
     position: relative;
     z-index: 0;
     height: 100%;
   }

   h1, h2, h3, h4, h5, h6, p {
     margin: 0;
   }

   body {
     font-family: 'Spoqa Han Sans Neo';
     font-size: 14px;
     -webkit-font-smoothing: antialiased;
     -moz-osx-font-smoothing: grayscale;
     color: #0a0a0a;
     letter-spacing: -0.057em;
     word-break: keep-all;
   }

   ul, ol {
     list-style: none;
     padding: 0;
     margin: 0;
   }

   a {
     text-decoration: none;
     cursor: pointer;
   }

  li.carousel__visible {
    h3, p {
      opacity: 1;
      transition: opacity 1s;
    }
  }

  li.carousel__hidden {
    h3, p {
      opacity: 0;
    }
  }
 `;

export default Component;
```
이후, app.page에서 위에서 만든 GlobalStyle 컴포넌트를 세팅 후 시작하면 된다.

대충 app.page 예시...

```tsx
  const CustomApp = ({ Component, pageProps }) => {
    return (
      <ThemeProvider theme={theme}>
      <GlobalStyle />
      <Head>
        <meta charset="utf-8" key="charSet" />
      </Head>
      <Component {...pageProps}>
      </ThemeProvider>
    )
  }

```