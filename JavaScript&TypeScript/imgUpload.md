## 이미지 업로드 구현하기
<br>
이미지 업로드를 기존에는 ref를 이용하지 않고, label의 htmlFor를 사용했었다.

하지만 이번엔 ref를 사용, input에 해당 ref를 걸어주었다.

주요 동작원리는 다음과 같다.

버튼을 클릭하면, onclick이벤트 내에서 input의 ref를 이용하여 input의 클릭 이벤트를 발생시킨다. 그렇게 하면 input type=’file’이 눌린 효과로 인해 파일 업로드 기능이 트리거 된다.

기존에는 미처 신경쓰지 못했던, accept를 이용(파일 확장자를 이용해 이미지만 받도록) 하였고, 파일 갯수 제한이나 파일 크기 제한도 처리해두는 로직을 구현하였다. 

파일 제한 갯수나 크기 등의  변수들은 props로 받을 수 있게 해 두었다. 

또한, 업로드한 이미지를 바로 보여줄 수 있도록 하는 처리도 해두었는데, 이 과정에서는 URL의 createObjectURL메서드를 이용하여 처리해주었다. 
그냥 File 객체에는 url이 없기 때문에, 이미지를 뿌려줄 수 있도록 Blob으로 변환하여 url을 추출하는 식으로 처리하였다.

그리고 한 가지 주의할 점은, createObjectURL 메서드를 이용하면

 메모리에 url주소가 등록되기 때문에, 지워주지 않으면 계속 남게 되므로 메모리 누수가 발생한다.

따라서 revokeObjectURL 메서드를 통해 create했던 url들을 지워주는 처리까지 해 두었다.

 전체코드

```tsx
import { FC, useRef, useState } from 'react';
import styled from 'styled-components';

import Button from '../Button';
import Image from '../Image';

type Props = {
  uploadFile: (data: File | FileList) => void;
  showImages?: boolean;
  isMultiple?: boolean;
  imageWidth?: string;
  imageheight?: string;
  uploadLimit?: number;
  maxFileSize?: number;
};

const Component: FC<Props> = ({
  uploadFile,
  showImages = false,
  isMultiple = false,
  imageWidth = '100px',
  imageheight = '100px',
  uploadLimit = 5,
  maxFileSize = 5242880, // 5MB 용량 제한
}) => {
  const inputRef = useRef<HTMLInputElement>(null);
  const [imgLoading, setImgLoading] = useState(false);
  const [images, setImages] = useState<string[]>([]);

  const handleClick = (e: React.MouseEvent<HTMLButtonElement, MouseEvent>) => {
    e.preventDefault();
    if (inputRef.current) {
      inputRef.current.click();
    }
  };

  const checkUploadLimit = (files: FileList) => {
    const totalImgs = Object.values(files).length + images.length;

    if (totalImgs > uploadLimit) {
      alert(`파일 업로드는 한 번에 최대 ${uploadLimit}개까지 가능합니다.`);
      return false;
    }

    return true;
  };

  const checkFileSize = (files: FileList) => {
    let result = true;
    const fileList = Object.values(files);
    fileList.forEach((item) => {
      const fileSize = Number(item.size);
      if (fileSize > maxFileSize) {
        alert('파일이 너무 큽니다.');
        result = false;
        return;
      }
    });
    return result;
  };

  const uploadImage = (event: React.ChangeEvent<HTMLInputElement>) => {
    setImgLoading(true);
    try {
      if (
        event.target.files &&
        checkUploadLimit(event.target.files) &&
        checkFileSize(event.target.files)
      ) {
        if (isMultiple) {
          const fileList = Object.values(event.target.files);
          const imgUrls: string[] = [];
          fileList.forEach((item) => {
            const url = URL.createObjectURL(item);
            imgUrls.push(url);
          });
          const newImgs = [...images].concat(imgUrls);
          setImages(newImgs);
        } else {
          uploadFile(event.target.files[0]);
          const url = URL.createObjectURL(event.target.files[0]);
          const imgs = [...images, url];
          setImages(imgs);
        }
      }
    } catch (err) {
      console.log(err);
    } finally {
      setImgLoading(false);
      if (images) {
        images.map((item) => {
          URL.revokeObjectURL(item);
        });
      }
    }
  };

  return (
    <Wrapper>
      <Container>
        <ImageUploadInput
          id="image-file-uploader"
          type="file"
          accept="image/*"
          multiple={isMultiple}
          ref={inputRef}
          onChange={uploadImage}
        />
        <StyledBtn
          width="70px"
          size="sm"
          label="업로드"
          variant="gray"
          isLoading={imgLoading}
          onClick={handleClick}
        />
      </Container>
      {showImages && images.length > 0 && (
        <ImageContainer>
          {images.map((imgSrc, idx) => (
            <ImageWrapper
              key={idx}
              imgWidth={imageWidth}
              imgHeight={imageheight}
            >
              <Image layout="fill" src={imgSrc} />
            </ImageWrapper>
          ))}
        </ImageContainer>
      )}
    </Wrapper>
  );
};

export default Component;

const Wrapper = styled.div``;

const Container = styled.div``;

type ImageWrapperProps = {
  imgWidth: string;
  imgHeight: string;
};
const ImageWrapper = styled.div<ImageWrapperProps>`
  position: relative;
  width: ${({ imgWidth }) => imgWidth};
  height: ${({ imgHeight }) => imgHeight};
  margin-top: 24px;
`;

const ImageContainer = styled.div`
  display: flex;
  align-items: center;

  & > ${ImageWrapper} + ${ImageWrapper} {
    margin-left: 12px;
  }
`;

const ImageUploadInput = styled.input`
  display: none;
`;

const StyledBtn = styled(Button)`
  border-radius: 5px;
`;
```