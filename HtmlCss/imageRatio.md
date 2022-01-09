## 이미지 비율 맞추기 (1대1 예시)
<br>

이미지의 실제 크기와 별개로, 이미지를 특정 비율로 유지해야 하는 경우가 있다. (특히 쇼핑몰에서 상품 목록 같은 경우)

보통 다음과 같은 요구조건이 있다.

- 박스 내에 빈 공간이 생기더라도 이미지의 비율은 유지되거나, 잘리더라도 좌측 상단이 아닌 중앙 부분이 보여야 한다.

- 브라우저의 크기가 줄어들더라도 박스의 비율은 그대로 유지돼야 한다.

이를 활용하는 방법은 relative, absolute를 활용하여 처리해줄 수 있다.

코드 예시는 다음과 같다.

```tsx
const ImageContainer = styled.div`
  position: relative;
  height: 0;
  padding-bottom: 100%;
  
`;

const ImageBox = styled.img`
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
`;
```

padding-top이 즉 높이를 나타내게 되는데, 100%라는 것은 즉 높이 100%라는 얘기이다. 만약 4대3비율을 원하면 70%로 설정하면 된다.