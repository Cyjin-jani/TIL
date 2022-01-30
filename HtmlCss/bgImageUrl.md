## background-image: url 제대로 사용하기
<br>

css 속성인 background-image: url();에는 url안에 string이 들어간다.

즉, 이미지 주소가 들어가게 되는데,

만약 이미지 주소가 아래와 같은 경우, 잘못된 속성값이라고 하여 터지는 경우가 발생한다. (이미지 안나옴)

<url> 타입의 경우:

문서내 CSS에 포함할 리소스에 대한 상대적 또는 절대 주소, 포인터인 url 또는 데이터 uri(선택적으로 단일 또는 큰따옴표로 묶음)이다. URL에 괄호, 백스페이스 또는 따옴표가 포함되어 있거나, 이러한 문자가 이스케이프(Escape) 하지 않는 한, 또는 주소가 0x7e를 초과하는 제어 문자를 포함하는 경우 인용 부호가 필요하다.

DATA URIs 리소스의 경우:

문서에 인라인 데이터를 제공하는 URI 체계로 HTML 및 CSS에 이미지를 내장하는 데 일반적으로 사용된다.

SVG 리소스의 경우:

SVG 구현 코드에서 리소스 ID를 사용한다.

내 경우 문제시 되었던 점은, imageUrl이 보통은 괄호를 포함하지 않지만, 이번에 특이하게 괄호를 포함해서 이미지가 안 나오게 되었다.

![Untitled (1)](https://user-images.githubusercontent.com/57857367/151688271-884b8827-011c-4027-8780-681fdef36f6b.png)

(느낌표가 뜨며 잘못된 속성값이라 뜸 → 이미지 못 그려줌)

해결 방안: 괄호가 포함된 걸 string으로 만들자

```tsx
const Image = styled.div<{ imageSource?: string }>`
  width: 100%;
  height: 100%;
  ${({ imageSource }) =>
    imageSource &&
    css`
      background-image: url('${imageSource}');
    `};
  background-size: cover;
  background-position: 50% 50%;
`;
```

기존 코드는 아래와 같았다....

```tsx
const Image = styled.div<{ imageSource?: string }>`
  width: 100%;
  height: 100%;
  background-image: ${({ imageSource }) => `url(${imageSource})`};
  background-size: cover;
  background-position: 50% 50%;
`;
```

위와 같이 되어있으면, undefined일 때도 문제가 됨

background-image: url("") 등 url() 괄호 안에 빈 값 등이 들어간다면, 불필요하게 현재 페이지의 서버를 재호출하게 된다.

그래서 차라리 정말 imageSource가 있는 경우에만 background-image를 세팅해주도록 바꾸었다.

참고 레퍼런스

[http://www.devdic.com/css/refer/functions/function:197/url()](http://www.devdic.com/css/refer/functions/function:197/url())