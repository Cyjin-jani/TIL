## input type file 같은 파일 업로드
<br>

input type=”file”의 onChange 함수를 이용하여 file 업로드를 핸들링하는데,

한 번 업로드 했던 파일은 다시 업로드 할 수 없는 문제가 있었다.

예를 들어 이미지1.jpg 파일을 업로드 한 뒤, 업로드 한 이미지가 마음에 들지 않아 삭제를 했다가,
다시 똑같은 파일을 업로드 하는 경우에는 사용자 입장에서 다시 업로드가 되어야 하는 것이 정상이다.

그러나, input의 입장에선 이미 onChange에서 해당 파일을 value로 가지고 있기에, 삭제여부와 상관없이

해당 value가 바뀌지 않았으므로(같은 파일이므로) 다시 업로드가 되지 않는다...

그래서 onChange에서 value를 리셋할 필요가 있었다.

코드 예시

```tsx
const uploadImage = (event: React.ChangeEvent<HTMLInputElement>) => {
    try {
      if (event.target.files) {
        uploadFile(event.target.files[0]);
        // 다시 같은 파일을 올릴 수 있도록 값을 초기화 합니다.
        event.target.value = '';
      }
    } catch (err) {
      console.log(err);
    } 
  };

// 중략
// JSX부분
<ImageUploadInput
  id="image-file-uploader"
  type="file"
  accept={acceptType}
  multiple={isMultiple}
  ref={inputRef}
  onChange={uploadImage}
/>
```

위 처럼 target.value를 초기화 해주니, 같은 파일을 지속해서 업로드할 수 있게 되었다.