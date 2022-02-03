## JS 경로를 새 창(탭)으로 띄우기
<br>

경로를 새 창으로 띄워야 하는 경우가 있다.

보통은 a태그만을 쓴다면, target:”_blank”를 사용할 수도 있을 것이다.

그러나 나의 경우에는, onClick 이벤트 함수의 내부 처리 과정 중 하나의 기능으로 새 탭을 띄웠어야 했다.

그래서 사용한 것이 window의 open이다.

사용법은 다음과 같다.

```tsx
// next js (ssr)이기에, window가 undefined일 수 있다.
if (window) {
  // 새 창으로 링크 페이지를 띄운다.
  window.open(selectedItem.linkUrl);
}
```

사실 위와 같이 간단하게 사용하는 방법 이외에  아래 name, specs, replace 등에 다양한 값을 넣어 원하는 동작을 구현할 수 있다.

```tsx
var ret = window.open(url, name, specs, replace);
```

name의 경우는 일반적인 string이 들어갈 수 있는데, 이는 새 창을 여는데, 창의 이름을 지정하는 부분이다.

이외에는 아래와 같은 값들이 존재한다.

- _blank: 기본값. 새 창(탭)에 열린다.
- _parent: 부모 프레임에 열린다.
- _self: 현재 페이지를 대체한다.
- _top: 로드된 프레임셋을 대체한다.

specs의 경우는 다양한 속성들을 통해 해당 창을 컨트롤 한다.

- **channelmode=yes|no|1|0** : 전체화면으로 창이 열린다. IE에서만 동작.
- **fullscreen=yes|no|1|0** : 전체 화면 모드. IE에서만 동작.
- **height=pixels** : 창의 높이를 지정(height=600)
- **width=pixels** : 창의 너비를 지정(width=500)
- **left=pixels** : 창의 화면 왼쪽에서의 위치를 지정한다. 음수는 사용할 수 없다.
- **top=pixels** : 창의 화면 위쪽에서의 위치를 지정한다. 음수는 사용할 수 없다.
- **menubar=yes|no|1|0** : 메뉴바 사용여부를 지정한다.
- **status=yes|no|1|0** : 상태바를 보여줄지 지정한다.
- **titlebar=yes|no|1|0** : 타이틀바를 보여줄지 지정한다. 호출 응용 프로그램이 HTML 응용 프로그램이거나 신뢰할 수있는 대화 상자가 아니면 무시.
- **location=yes|no|1|0** : 주소 표시줄 사용여부를 지정한다. Opera에서만 동작.
- **resizable=yes|no|1|0** : 창의 리사이즈 가능 여부를 지정한다. IE에서만 동작.
- **scrollbars=yes|no|1|0** : 스크롤바 사용여부를 지정한다. IE, Firefox, Opera에서 동작.
- **toolbar=yes|no|1|0** : 툴바를 보여줄지 지정한다. IE, Firefox에서 동작.

replace의 경우는 히스토리의 목록에 새 항목을 추가할 지 여부를 판단한다.

- true: 현재 히스토리 대체한다.
- false: 히스토리에 새 항목을 만든다.

출처:

[https://offbyone.tistory.com/312](https://offbyone.tistory.com/312)