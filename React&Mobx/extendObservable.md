## mobx의 extendObservable
<br>

mobx의 기능 중 하나인 extendObservable에 대해 알아보자.

회사 코드를 보던 중, 아래와 같은 코드를 발견했다.

```tsx
export interface TestJson {
  id: string;
  quantity?: number;
}

export default class TestModel implements TestJson {
  _id: string;

	@observable
  quantity?: number;

  constructor() {
    extendObservable(this, { ...this });
  }
}
```

이미 quantity 필드에 observable이 붙어 있었지만, 추가적으로 아래 생성자 부분에 extendObservable이 있는 걸 보고, 어떤 기능을 해주는 코드인지 궁금증이 생겼다.

일단 공식 문서의 설명을 보면,

대충 타겟 객체에 새로운 프로퍼티들을 즉각적으로 observable한 것으로 만들어준다는 듯 하다.

> Can be used to introduce new properties on the `target` object and make them observable immediately. Basically a shorthand for `Object.assign(target, properties); makeAutoObservable(target, overrides, options);`. However, existing properties on `target` won't be touched.
> 

다만, 무조건 영구적으로 observable하게 만드는 것은 아닌 듯 하다.

원본 target의 속성을 건드리지(변화시키지) 않고 observing이 가능하도록 해주는 것이라고 보면 될 듯 하다.

인스턴스를 할당해서 사용할 때만 적용되는 것 같다.

아래는 간단한 사용 예시이다.

```tsx
import React, { Component } from 'react';
import { render } from 'react-dom';
import Hello from './Hello';
import './style.css';
import { extendObservable } from 'mobx'
import { observer } from 'mobx-react'

class HelloWorld {
  constructor(params) {
    extendObservable(this, params)
  }

}

const helloWorld = new HelloWorld({
  firstName: 'Basilio',
  lastName: 'Poupkine'
})

@observer
class App extends Component {
  constructor() {
    super();
    this.state = {
      name: 'React'
    };
  }

  handleChange = ({target}) => helloWorld[target.name] = target.value

  render() {
    return (
      <div>
        <Hello name={this.state.name} />
        <p>
          Start editing to see some magic happen :)
        </p>
        <p>First Name: { helloWorld.firstName }</p>
        <p>Last Name: { helloWorld.lastName }</p>
        <input value={helloWorld.firstName } name="firstName" onChange={ this.handleChange } />
        <input value={helloWorld.lastName } name="lastName" onChange={ this.handleChange } />
      </div>
    );
  }
}

render(<App />, document.getElementById('root'));
```

참고

[https://ko.mobx.js.org/api.html#extendobservable](https://ko.mobx.js.org/api.html#extendobservable)