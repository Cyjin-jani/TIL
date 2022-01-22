## 리액트에서 key의 중요성 (index 멈춰!)
<br>

흔히 list형태의 데이터를 map을 이용하여 렌더링을 해 줄 때, 

key의 중요성을 간과하고 index 등을 넣는 경우가 있는데 (혹은 아예 넣지 않거나;;)

이는 매우 지양해야 한다. 왜냐하면 리액트에서는 key값을 통해 렌더링되는 요소를 구분하기 때문이다.

예를 들어, 아래와 같은 경우 심각한 버그를 발생시킨다.

```tsx
{selectedOptions.map((option, index) => (
  <CounterBox key={index}>
    <Counter
      title={option.variantName || title}
      discountedPrice={discountedPrice}
    />
  </CounterBox>
))}
```

이 같은 경우, options에 따라 counter가 여러가지 생기는데, 해당 counter는 상품의 수량을 가지고 있다.

데이터는 멀쩡히 특정 상품의 특정 수량을 가지고 있지만, key가 index인 경우에, 

예를 들어 0번째 상품 개수 3개, 1번째 상품 개수 2개 로 만든뒤, 0번째를 삭제하는 경우,

당연히 options 내 데이터 또한 1번째 상품 개수 2개로 남아있게 되는데

실제로 렌더링 된 카운터 부분은 숫자 3이 그대로 남아있다!!

왜냐하면 key=0인경우 3개였는데  0번째를 삭제했음에도 map돌면서 다시 index가 0이 존재하기 때문에 그대로 3이 남아있게 된다.

따라서 아래 코드와 같이 key를 확실한 구별자인 옵션의 name으로 변경해주자, key가 확실히 다르므로 0번째를 삭제하면 카운터가 2로 남아있게 된다.

```tsx
{selectedOptions.map((option) => (
  <CounterBox key={option.variantName}>
    <Counter
      title={option.variantName || title}
      discountedPrice={discountedPrice}
    />
  </CounterBox>
))}
```

만약, 확실한 구별자가 없는 경우엔 어떻게 해야하나?

랜덤한 id값을 string으로 제공하는 외부 라이브러리를 사용하여야 한다... 

(uuid 혹은 nanoid 등을 사용하자...)

물론 아래와 같은 경우에는, index를 사용해도 무방하다... (하지만 코드의 통일성을 위해 사용하지 않는게 좋음...)

- **배열과 각 요소가 수정, 삭제, 추가 등의 기능이 없는 단순 렌더링만 담당하는 경우**
- **id로 쓸만한 unique 값이 없을 경우**
- **정렬 혹은 필터 요소가 없어야 함**