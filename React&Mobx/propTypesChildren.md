## propTypes children 정의하기
<br>
children props의 경우,

propTypes로는 어떻게 정의해야 하는지 헷갈릴 수 있다.

보통은 propTypes.element라고 생각할 수 있으나, 

그렇게만 정의 한다면, 아래와 같이 사용시 낭낭하게 워닝이 뜨는 걸 볼 수 있다.

사용 예시

```jsx
// Section의 children은 propTypes.element이다.
<Section>3</Section>

// Section.js에서.
Section.propTypes = {
  children: PropTypes.element,
};
```

워닝

```jsx
Warning: Failed prop type: Invalid prop `children` of type `string` 
supplied to `Component`, expected a single ReactElement
```

node가 아니라 element이기에 자연스레 텍스트 노드가 children으로 들어가면 에러가 난다.

따라서 일반적으로 생각하면 Proptypes.node로 타입을 변경하면 된다.

하지만 단일 노드가 아닌, 여러 종류의 element 혹은 node가 포함될 수 있으므로

최종적으로는 아래와 같은 형태를 띄면 될 것이다.

최종 코드

```jsx
children: PropTypes.oneOfType([
    PropTypes.arrayOf(PropTypes.node),
    PropTypes.node,
  ]),
```

이렇게 설정하자, 해당 컴포넌트의 children으로 어떤 종류의 노드를 집어넣던, 워닝이 뜨지 않게 되었다.

참고

[https://github.com/jsx-eslint/eslint-plugin-react/issues/7](https://github.com/jsx-eslint/eslint-plugin-react/issues/7)

[http://daplus.net/reactjs-reactjs-this-props-children의-proptype은-무엇입니까/](http://daplus.net/reactjs-reactjs-this-props-children%EC%9D%98-proptype%EC%9D%80-%EB%AC%B4%EC%97%87%EC%9E%85%EB%8B%88%EA%B9%8C/)