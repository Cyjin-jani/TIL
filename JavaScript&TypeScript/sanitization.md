## innerHtml과 Sanitization
<br>

자바스크립트를 사용하다 보면, innerHTML을 사용해 본 경험이 있을 것이다.

innerHTML을 사용한 DOM 조작은 구현이 비교적 간단하고 직관적이라는 장점이 있으나, 크로스사이트 스크립팅이라는 공격에 취약하다는 단점이 있다.

### 크로스 사이트 스크립팅이란?

웹 애플리케이션에서 많이 나타나는 취약점의 하나로 웹사이트 관리자가 아닌 이가 웹 페이지에 악성 스크립트를 삽입할 수 있는 취약점이다.

자세한 내용은 아래 블로그 참고

[https://daniel-park.tistory.com/45](https://daniel-park.tistory.com/45)

### 해결방법 - 새니티제이션

html 새니티제이션(Sanitization)은 사용자로부터 입력받은 데이터에 의해 발생할 수 있는 크로스 사이트 스크립팅 공격을 예방하기 위해 잠재적 위험을 제거하는 기능을 말한다.

해당 함수를 직접 구현할 수도 있지만, DOMPurify 라는 라이브러리를 사용하기를 deep-dive 책에서는 권장하고 있다. (해당 라이브러리는 2014년 2월부터 제공된 나름 안정성 보장된 라이브러리이다.)

사용 예시

```tsx
DOMPurify.sanitize('<img src="/someWhere" onerror="alert(document.cookie)">');
```

그 이외에...

innerHTML과 비슷한 기능을 하는 메서드인 insertAdjacentHTML 메서드가 있다.

이 메서드 역시 크로스 사이트 스크립팅 공격에 취약하다.