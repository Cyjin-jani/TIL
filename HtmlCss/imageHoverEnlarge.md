## image hover 효과주기 (확대 / 축소)
<br>

이미지에 마우스 hover를 하면, 조금 확대시켜 보여주고, 다시 blur가 되면 원래크기로 돌아가는 효과

이미지의 크기를 확대하는 방법으로는 transform: scale(n)이 존재한다. 좀 더 부드럽게 확대하고 싶다면 transition을 같이 사용하는 것이 좋다.

이미지를 담고 있는 외부(부모) container에 overflow: hidden을 주면 된다.

종합적인 코드는 아래와 같다.  (아래 이미지는 1대1 정사각형 비율 고정)

```tsx
const ImageWrapper = styled.div`
  position: relative;
  height: 0;
  padding-bottom: 100%;
  margin-bottom: 15px;
  overflow: hidden;
  @media screen and (max-width: 720px) {
    margin-bottom: 8px;
  }
  border-radius: 4px;
  @media screen and (max-width: 720px) {
    border-radius: 2px;
  }
`;

const StyledImage = styled(Image)<ImageProps>`
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;

  border-radius: 4px;
  @media screen and (max-width: 720px) {
    border-radius: 2px;
  }

  &:hover {
    transform: scale(1.05);
    transition: all 0.2s linear;
  }
  &:not(:hover) {
    transform: scale(1);
    transition: all 0.2s linear;
  }
`;

// Image component
import { FC } from 'react';
import styled from 'styled-components';

const Image = styled.img``;

type Props = {
  src: string;
  alt?: string;
  onClick?: () => void;
};

const Component: FC<Props> = ({ src, alt, onClick, ...rest }) => {
  return <Image src={src} title={alt} {...rest} onClick={onClick} />;
};

export default Component;
```
