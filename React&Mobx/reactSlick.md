## react-slick을 이용하여 캐로셀 만들기
<br>
react-slick의 점유율이 압도적이므로 한 번 사용해 보자.

그동안은 pure-react-carousel을 썼었으나…

[https://www.npmtrends.com/nuka-carousel-vs-pure-react-carousel-vs-react-id-swiper-vs-react-slick-vs-react-slider-vs-react-swipe-vs-react-swipeable-vs-react-swiper-vs-react-slideshow-carousel](https://www.npmtrends.com/nuka-carousel-vs-pure-react-carousel-vs-react-id-swiper-vs-react-slick-vs-react-slider-vs-react-swipe-vs-react-swipeable-vs-react-swiper-vs-react-slideshow-carousel)

너무나도 압도적인 1위….

공식문서는 아래 주소로 들어가면 된다.

[https://react-slick.neostack.com/](https://react-slick.neostack.com/)

일단 npm 패키지 설치는 아래 두 가지를 설치해주어야 한다.

```bash
npm install react-slick --save
npm install slick-carousel --save
```

사용법은 다음과 같이 소개되고 있다.

```jsx
import React, { Component } from "react";
import Slider from "react-slick";

import "slick-carousel/slick/slick.css"; 
import "slick-carousel/slick/slick-theme.css";

export default class SimpleSlider extends Component {
  render() {
    const settings = {
      dots: true,
      infinite: true,
      speed: 500,
      slidesToShow: 1,
      slidesToScroll: 1
    };
    return (
      <div>
        <h2> Single Item</h2>
        <Slider {...settings}>
          <div>
            <h3>1</h3>
          </div>
          <div>
            <h3>2</h3>
          </div>
          <div>
            <h3>3</h3>
          </div>
          <div>
            <h3>4</h3>
          </div>
          <div>
            <h3>5</h3>
          </div>
          <div>
            <h3>6</h3>
          </div>
        </Slider>
      </div>
    );
  }
}
```

(주의. css를 import할 때, ~를 쓰면 제대로 import를 못 하는 경우가 생긴다. 경우에 따라서는 import from구문의 물결(~)을 제거해주고 사용해야 한다.)

커스터마이징은 의외로 크게 어렵지는 않다.

공식문서에 자세한 설명과 예제가 나와있어, 왠만하면 예제를 보면서 조금씩 바꿔주면 된다.

아래는 내가 현재 진행중인 프젝에 맞도록 고친 부분이다.
커스터마이징에 참고하면 좋다.

```jsx
import PropTypes from 'prop-types';
import Slider from 'react-slick';
import styled from 'styled-components';

import PrevArrowIcon from './icons/ic-left.svg';
import NextArrowIcon from './icons/ic-right.svg';

import 'slick-carousel/slick/slick.css';
import 'slick-carousel/slick/slick-theme.css';

const Component = ({ children }) => {
  function CarouselNextArrow(props) {
    const { className, onClick } = props;
    return (
      <ArrowWrapper
        className={className}
        onClick={onClick}
        style={{ right: 0 }}
      >
        <NextArrowIcon />
      </ArrowWrapper>
    );
  }
  function CarouselPrevArrow(props) {
    const { className, onClick } = props;
    return (
      <ArrowWrapper
        className={className}
        onClick={onClick}
        style={{ left: 0, zIndex: 1 }}
      >
        <PrevArrowIcon />
      </ArrowWrapper>
    );
  }

  const settings = {
    dots: true,
    infinite: true,
    fade: false,
    lazyLoad: true,
    speed: 500,
    slidesToShow: 1,
    slidesToScroll: 1,
    nextArrow: <CarouselNextArrow />,
    prevArrow: <CarouselPrevArrow />,
    appendDots: (dots) => (
      <DotListWrapper>
        <DotUl> {dots} </DotUl>
      </DotListWrapper>
    ),
  };

  return <StyledSlider {...settings}>{children}</StyledSlider>;
};

const StyledSlider = styled(Slider)`
  .slick-dots {
    bottom: -18%;
  }

  .slick-dots li {
    margin: 0;
  }

  .slick-dots li button:before {
    opacity: 1;
    color: #e5e5e8;
  }
  .slick-dots li.slick-active button:before {
    color: black;
  }
`;

const ArrowWrapper = styled.div`
  display: flex;
  top: calc(50% + 56px);
  transform: translateY(calc(-50% - 32px));
  width: 40px;
  height: 56px;
  justify-content: center;
  align-items: center;
  background-color: rgba(0, 0, 0, 0.8);

  &:before {
    display: none;
  }

  &:hover {
    background-color: rgba(0, 0, 0, 0.8);
  }

  @media screen and (max-width: 768px) {
    width: 32px;
    height: 48px;
    transform: translateY(calc(-50% - 80px));
  }
`;

const DotListWrapper = styled.div`
  padding: 10px;
`;

const DotUl = styled.ul`
  margin: 0;
`;

Component.propTypes = {
  children: PropTypes.oneOfType([
    PropTypes.arrayOf(PropTypes.node),
    PropTypes.node,
  ]),
};

export default Component;
```