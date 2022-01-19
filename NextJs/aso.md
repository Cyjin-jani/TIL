## nextjs에서의 automatic static optimization
<br>

회사에서는 app.js에 customApp으로 재구성 후 getInitialProps를 이용하여 필요한 데이터 세팅을 해주고 있다.

next js 공식 문서상에서는 app.js에서 getInitialProps를 사용하면, automatic static optimization이 동작하지 않는다.

> If you have a [custom `App`](https://nextjs.org/docs/advanced-features/custom-app) with `getInitialProps` then this optimization will be turned off in pages without [Static Generation](https://nextjs.org/docs/basic-features/data-fetching/get-static-props).
> 

얼핏 들으면 뭔가 해서는 안 될것 같은 무서운(?) 말이지만, 사실 정적 페이지 사전 렌더링이 필요한 경우가 아닌 이상,  getInitialProps를 사용해도 크게 문제되지 않는다.

자동으로 정적 페이지 최적화를 하게 된다면 next build 시에 아래와 같이 변한다.

예를 들어, pages/about.js 가 있다면, build시 아래와 같이 정적 최적화를 해준다.

```tsx
.next/server/pages/about.html
```

반면, getServersideProps 등이 있다면, build 시에 아래와 같이 빌드된다.

```tsx
.next/server/pages/about.js
```

* 주의* 여기서 js로 변한다고 해서, 서버사이드 렌더링이 안된다는 의미는 아니다!

주로 getInitialProps는 프로젝트에서 공통적으로 필요한 data를 사전에 가져오기 위해 사용되곤 한다.

그로 인한 장점은 다음과 같다.

1. **속도가 빨라진다.** 

브라우저에서의 연산을 서버와 함께 하면서 미리 데이터를 받아오고 브라우저는 렌더링만 할 수 있기 때문이다. 

2. **코드 상의 처리가 깔끔**해진다. 

데이터가 꼭 필요한 페이지의 경우 브라우저가 데이터를 가져올 때까지 화면 렌더링을 잠시 null 처리(혹은 로딩 처리 등)하는 경우가 있다. 이 과정이 없어지고, 이니셜라이징한 데이터가 들어오는 과정을 전제로 코드를 작성할 수 있다.

주의점은, 서버에서 동작하는 로직이므로, client에서만 가능한 로직은 피해야 한다! (죄다 undefined일 거니..)

참고

[https://nextjs.org/docs/advanced-features/automatic-static-optimization](https://nextjs.org/docs/advanced-features/automatic-static-optimization)

[https://velog.io/@cyranocoding/Next-js-구동방식-과-getInitialProps#getinitialprops](https://velog.io/@cyranocoding/Next-js-%EA%B5%AC%EB%8F%99%EB%B0%A9%EC%8B%9D-%EA%B3%BC-getInitialProps#getinitialprops)