## useRef 활용하기
<br>

useRef는 순수 자바스크립트 객체로서, DOM 뿐만이 아니라 어떤 값이든 저장할 수 있다. 한 번 저장된 값을 유지하고 있으며, 매번 렌더링될 때에도 기존의 값을 유지한다.

또한, 값이 변하게 되어도 리렌더링이 되지 않는다는 특징이 있다.

사용 예시는 다음과 같다.

내가 만든 검색 input 컴포넌트인데, 사용자가 검색 인풋을 클릭하면, 검색 버튼이 진하게 표시되고, 포커스가 안된 경우에는 흐리게 보여줘야 했다....
결국 focus가 되었는지, blur가 되었는지 이벤트 발생시에 처리를 해줬어야 했기에 아래와 같이 useRef를 사용하여 해결해주었다.


```tsx

import { FC, useEffect, useRef, useState } from 'react';
import styled, { css } from 'styled-components';

import Image from '~presentation/Image';

type Props = {
  handleSearch: (value: string) => void;
  handleInit: boolean;
};

const Component: FC<Props> = ({ handleSearch, handleInit }) => {
  const [inputValue, setInputValue] = useState('');
  const [isFocused, setIsFocused] = useState(false);

  const inputRef = useRef<HTMLInputElement>(null);

  useEffect(() => {
    if (inputRef.current) {
      inputRef.current.onfocus = function (e) {
        setIsFocused(true);
      };
      inputRef.current.onblur = function (e) {
        setIsFocused(false);
      };
    }
  }, []);

  useEffect(() => {
    if (handleInit) {
      setInputValue('');
    }
  }, [handleInit]);

  const onChange = (e: React.ChangeEvent<HTMLInputElement>) => {
    setInputValue(e.target.value);
  };

  const handleClick = () => {
    handleSearch(inputValue);
  };

  const handleReset = () => {
    setInputValue('');
  };

  return (
    <Wrapper>
      <InputBox>
        <SearchInput
          ref={inputRef}
          type="text"
          value={inputValue}
          placeholder="체험단 검색"
          onChange={onChange}
        />
      </InputBox>
      {inputValue && (
        <ResetBtn onClick={handleReset}>
          <Image src="/images/resetInput.svg" />
        </ResetBtn>
      )}
      <SearchBtn $isFocused={isFocused} onClick={handleClick}>
        <Image src="/images/search.svg" />
      </SearchBtn>
    </Wrapper>
  );
};

export default Component;

const Wrapper = styled.div`
  display: flex;
  width: 232px;
  height: 48px;
  padding: 12px 20px;
  background: #f8f8fa;
  border-radius: 24px;
  align-items: center;

  @media screen and (max-width: 768px) {
    width: 100%;
  }
  @media screen and (max-width: 720px) {
    padding-left: 12px;
  }
`;

const InputBox = styled.div`
  width: 100%;
  height: 32px;
  margin-right: 8px;
`;

const SearchInput = styled.input`
  width: 100%;
  height: 32px;
  border: none;
  background: #f8f8fa;
  outline: none;

  font-style: normal;
  font-weight: 500;
  font-size: 16px;
  line-height: 24px;
  letter-spacing: -0.05em;

  margin-top: 1px;

  &::placeholder {
    font-weight: 500;
    font-size: 16px;
    line-height: 24px;
    letter-spacing: -0.05em;
    color: #999999;
  }
`;

const SearchBtn = styled.div<{ $isFocused: boolean }>`
  cursor: pointer;
  opacity: ${({ $isFocused }) => ($isFocused ? '100%' : '40%')};
  transition: all 0.2s linear;
`;

const ResetBtn = styled.div`
  margin-right: 8px;
  cursor: pointer;
`;
```