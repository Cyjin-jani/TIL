## Snb(Gnb) active 효과 시 경로 판별하는 법
<br>

보통 gnb 혹은 snb를 만들게 되면, 

특정 경로(path)에 알맞게 snb 메뉴에 active 효과를 주게 된다.

다만, pathname 전체를 가지고 판별을 하게 되면, 제대로 동작하지 않는 경우가 생기는데, 디렉토리 구조가 복잡해 지는 경우가 바로 그렇다.

예를 들어, "/myProfile", "/cart" 등은 쉽게 판별이 되지만,

"/products/all", "/products/soldout" 처럼 디렉토리 구조가 한 번 더 들어가게 되면, 두 경로 모두 products이므로 active효과가 적용되야 하지만 일부만 적용되거나, 아예 적용되지 않는다.

이러한 경우, 경로(path)를 자체적으로 판별하여야 하는데,
그때 사용 가능한 방법은 물론 여러가지 존재하겠지만, 아래와 같이 2가지 방법이 있다.

아래 예시에서는 경로를 href로 표시하였다.

href를 자르는 방법 1 - slice 사용

```tsx
let activeLink = href;
  if (href.lastIndexOf('/') > 0) {
    activeLink = href.slice(0, href.lastIndexOf('/'));
  }
  const active = isActive
    ? isActive
    : router.asPath.startsWith(activeLink)
    ? true
    : false;
```

href를 자르는 방법 2 - substr 사용

```tsx
const index = href.lastIndexOf('/');
  const findActiveHref = index !== 0 ? href.substr(0, index) : href;
  const router = useRouter();
  const active = isActive
    ? isActive
    : router.asPath.startsWith(findActiveHref)
    ? true
    : false;
```
