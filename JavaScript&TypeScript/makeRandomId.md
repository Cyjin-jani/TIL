## JS 랜덤한 아이디 만들기
<br>

외부 패키지를 이용하지 않고, 간단하게 유니크한 아이디를 생성하는 코드이다.

보통 랜덤한 5자리숫자를 하는 경우도 있지만, Date를 활용하면 더욱 좋다.

```jsx
export function getUniqueId() {
  return Math.floor(Math.random() * Date.now()).toString();
}
```