## js로 스택 구현해보기
<br>

스택 자료형의 경우, Last-in-First-out 형태를 띈다.
스택의 연산은 push, pop으로 데이터를 꺼내고 넣을 수 있다.

또한, top 같은 경우는 pop과는 다르게 데이터를 삭제하지는 않고 맨 마지막 (가장 위)에 위치한 원소를 반환한다.

기타 자세한 설명은 다음 위키를 참고하자.

https://ko.wikipedia.org/wiki/스택

자바스크립트에서는 array로 스택을 구현할 수 있다.

```js
  class Stack {
    constructor() {
      this.arr = [];
    }

    push(x) {
      this.arr.push(x);
    }
    pop(index) {
      // 만약 맨 마지막 index라면 그대로 pop을 수행한다. (혹은 아무 index값도 넣지 않았다면)
      if (index === this.arr.length - 1 || index === undefined) {
        return this.arr.pop();
      }
      // 특정 순서에 위치한 값을 pop하고 싶은 경우
      let result = this.arr.splice(index, 1);
      return result;
    }
    isEmpty() {
      if(arr.length == 0) {
        // 비어있다면 1을 반환한다.
        return 1;
      } else {
        return 0;
      }
    }
    top() {
      return this.arr[this.arr.length - 1];
    }

    bottom() {
      return this.arr[0];
    }
}

  const stack = new Stack();
  stack.push(10);
  stack.push(20);
  stack.push(30);
  stack.push(40);
  console.log(stack.pop()); //40
  console.log(stack.pop(1)); // 20
  console.log(stack.top()); // 30
  console.log(stack.bottom()); // 10
  console.log('stack', stack);
```