## JS Symbol에 대해
<br>
Symbol이란,

심벌(Symbol)은 ES6에서 도입된 7번째 데이터 타입으로 **변경 불가능한 원시 타입의 값**이다. 

**주로 이름의 충돌 위험이 없는 유일한 프로퍼티 키를 만들기 위해 사용한다.**

Symbol의 사용법

심벌의 사용법은 단순하다. Symbol()이라는 키워드를 사용하면 되며, new 연산자는 필요없다. (생성자 함수가 아니므로)

```tsx
// Symbol 함수를 호출하여 유일무이한 심벌 값을 생성한다.
const mySymbol = Symbol();
console.log(typeof mySymbol); // symbol

// 심벌 값은 외부로 노출되지 않아 확인할 수 없다.
console.log(mySymbol); // Symbol()

// 심벌 값에 대한 설명(description)이 같더라도 유일무이한 심벌 값을 생성한다.
const mySymbol1 = Symbol("mySymbol");
const mySymbol2 = Symbol("mySymbol");

console.log(mySymbol1 === mySymbol2); // false

// 심벌도 레퍼 객체를 생성한다
console.log(mySymbol.description); // mySymbol
console.log(mySymbol.toString()); // Symbol(mySymbol)
```

Symbol의 활용

- Symbol을 전역에서 사용하고 싶은 경우

symbol을 전역에서 사용하고자 한다면 조금 다른 방식으로 심벌 값을 생성해야 한다. 

또한 생성한 전역 symbol의 호출시에도 Symbol의 메서드를 이용하여야만 한다.

```tsx
// 전역 심벌 레지스트리에 mySymbol이라는 키로 저장된 심벌 값이 없으면 새로운 심벌 값을 생성
const s1 = Symbol.for("globalSymbol");
// 전역 심벌 레지스트리에 저장된 심벌 값의 키를 추출
Symbol.keyFor(s1); // -> globalSymbol

// Symbol 함수를 호출하여 생성한 심벌 값은 전역 심벌 레지스트리에 등록되어 관리되지 않는다.
const s2 = Symbol("newSymbol");
// 전역 심벌 레지스트리에 저장된 심벌 값의 키를 추출
Symbol.keyFor(s2); // -> undefined
```

- Symbol의 활용법 - 일반적으로 객체의 프로퍼티 키로 사용된다.

*주의* dot 연산자는 사용이 불가하다.

```tsx
const mySymbols = Symbol("some car description");
const myObject2 = { name: 'bmw' };
myObject2[mySymbols] = 'This is a car';
console.log(mySymbols) // Symbol(some car description)
console.log(myObject2[mySymbols]) // This is a car
console.log(myObject2.mySymbols);// x
```

Symbol의 은닉화

Symbol로 추가한 객체의 프로퍼티들은 Object의 Keys나 for-in 문으로는 보여지지 않는다.

그러므로 은닉화가 어느정도 가능하다.

```tsx
const myObject = {};
myObject["prop1"] = 1;
myObject["prop2"] = 2;
const prop3 = Symbol("prop3");
const prop4 = Symbol("prop4");
myObject[prop3] = 3;
myObject[prop4] = 4;
for (const key in myObject){
  console.log(key); // prop3 prop4는 나오지 않음.
}
console.log(myObject[prop3]) // 3
console.log(myObject[prop3]) // 4
```

그러나 아래 두 개면 Symbol의 키와 값이 노출이 된다.

Object.getOwnPropertySymbols과 Reflect.ownKeys 메서드이다.

```tsx
// getOwnPropertySymbols 메서드는 인수로 전달한 객체의 심벌 프로퍼티 키를 배열로 반환한다.
console.log(Object.getOwnPropertySymbols(myObject)); 
// [Symbol(prop3), Symbol(prop4)]

// getOwnPropertySymbols 메서드로 심벌 값도 찾을 수 있다.
const symbolKey1 = Object.getOwnPropertySymbols(myObject)[0];
console.log(myObject[symbolKey1]); // 3
```

Symbol의 유용성

Symbol은 위에서 본 바에 의하면 큰 장점이 없다고 생각할 수 있다.

그러나 필자의 생각에 Symbol의 최고 장점은 아래처럼 객체에 중복되지 않는 고유한 프로퍼티를 추가할 수 있다는 것이다. 실제로 아래와 같이 함수를 값으로 할당해주면 원본 객체는 건드리지 않고 근사한 객체 내부 메서드가 생기는 것이다.

```tsx
const includes = Symbol('즐거운 자바스크립트');
Array.prototype[includes] = function () {
  return console.log('its Symbol');
}
const arr = [1, 2, 3];
arr.includes(1); // true
arr['includes'](1); // true
arr[includes](); // its Symbol
```

참고

[https://ui.toast.com/weekly-pick/ko_20210413](https://ui.toast.com/weekly-pick/ko_20210413)

[https://ko.javascript.info/symbol](https://ko.javascript.info/symbol)

[https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Reflect](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Reflect)

[https://medium.com/@hyunwoojo/javascript-symbol-에-대해서-6aa5903fb6f1](https://medium.com/@hyunwoojo/javascript-symbol-%EC%97%90-%EB%8C%80%ED%95%B4%EC%84%9C-6aa5903fb6f1)