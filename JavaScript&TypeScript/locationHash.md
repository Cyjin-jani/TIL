## location hash 사용하기
<br>

location의 hash에 값(string)을 저장해둘 수 있다.

```tsx
location.hash = 'something';
```

이를 활용하여 url에 hash값이 있으면, 특정 기능을 수행할 수 있다.

아래 예시는 list 컴포넌트에서 특정 listItem의 정보를 담은 modal을 띄워줄 때, hash에 해당 listItem의 아이디를 저장한다.

모달이 열린 상태에서, 새로고침 등을 해도 모달이 열린 상태를 유지하는 기능을 구현하였다.

listItem의 아이디를 저장해두고, 새로고침을 하면 해당 아이디에 맞는 listItem의 정보를 세팅 및 모달을 열어주는 행위이다.

코드는 다음과 같다.

```tsx
const handleClose = () => {
    if (location) {
      location.hash = '';
    }
    hideModal();
  };

useEffect(() => {
  if (location && location.hash) {
    const savedId = location.hash.substring(1);
    const findOne = notificationList.find((item) => item.id === savedId);
    if (findOne) {
      setSelectedItem(findOne);
      showModal();
    }
  }
}, []);
```

모달을 닫을 때, location.hash의 값을 초기화 해준다. 그렇지 않으면 해쉬값이 계속 남아있게 되고 그러면 결국 모달은 영원히 닫히지 않고 계속 열릴 것이기 때문이다.