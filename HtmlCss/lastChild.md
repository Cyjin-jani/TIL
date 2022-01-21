## last-child 제대로 사용하기
<br>

보통 반복되는 list 형태의 요소가 나열되는 경우, 각 반복되는 요소에 margin이나 divider 같은 요소를 주고, 맨 마지막에만 적용이 되지 않도록 하는 styling이 많다.

그런 경우에 쉽게 사용 가능한 css의 속성이 바로 :last-child이다.

사용 예시는 다음과 같다.

```html
<ul>
  <li>Item 1</li>
  <li>Item 2</li>
  <li>Item 3
    <ul>
      <li>Item 3.1</li>
      <li>Item 3.2</li>
      <li>Item 3.3</li>
    </ul>
  </li>
</ul>
```

```css
ul li {
  color: blue;
}

ul li:last-child {
  border: 1px solid red;
  color: red;
}
```

위와 같이 li:last-child를 해주면, ul 태그의 자식 중 반복되는 li들 중에 맨 마지막 친구에게만 border와 color를 주겠다는 의미이다.

다만, 한 가지 주의사항이 있다.

반복되는 요소에 last-child를 사용하기 위해서는 반복되는 요소들을 감싸는 하나의 parent 요소가 필요하고, 그 parent요소에 반복되는 요소 이외의 다른 요소가 있으면 안된다는 점이다!

회사에서 단순하게 listData를 map 돌려서 렌더링 하면서, margin-bottom을 주고, 맨 마지막에만 margin을 없애는 식으로 코딩을 하였다.

적용이 잘 되던 코드

```tsx
const Wrapper = styled.div``;
const ArticleItemWrapper = styled.div`
  margin-bottom: 12px;
  &:last-child {
    margin-bottom: 0;
  }
`;

//... 중략

return (
    <Wrapper>
        {notificationList.map((item) => (
          <ArticleItemWrapper key={item.id}>
            <NotificationListItem item={item} handleClick={handleClick} />
          </ArticleItemWrapper>
        ))}
    </Wrapper>
  );
```

갑자기 적용이 안 되는 코드

```tsx
const Wrapper = styled.div``;
const ArticleItemWrapper = styled.div`
  margin-bottom: 12px;
  &:last-child {
    margin-bottom: 0;
  }
`;

//... 중략

return (
    <Wrapper>
        {notificationList.map((item) => (
          <ArticleItemWrapper key={item.id}>
            <NotificationListItem item={item} handleClick={handleClick} />
          </ArticleItemWrapper>
        ))}

			<LoadMoreSection>
        <Button
          onClick={handleLoadMore}
          label="목록 더보기"
          variant="gray"
          size="md"
          width="240px"
          isLoading={vm.isLoading}
        />
      </LoadMoreSection>
    </Wrapper>
  );
```

<LoadMoreSection>을 추가하기 전 까지는 잘 적용되어 있어서

당연히 잘 되겠지 싶었는데,,,,, 적용이 왠걸... 안 되고 있었다!

보니까 Wrapper로 감싸져 있기는 하지만 LoadMoreSection이 있어서, 아마 last-child로 인식이 안되는 듯 하다.

mdn의 정의를 살펴보면, 다음과 같다.

> The **`:last-child`** [CSS](https://developer.mozilla.org/en-US/docs/Web/CSS) [pseudo-class](https://developer.mozilla.org/en-US/docs/Web/CSS/Pseudo-classes) represents the last element among a group of sibling elements.
> 

아마 같은 Wrapper안에 묶여 있었기에, group으로 포함이 되었고, 그 이야기인 즉슨, 자매 엘리먼트들 (sibling elements)중에서 last element는 ArticleItemWrapper가 아니라 LoadMoreSection이 된다는 이야기였다.

그렇기에, 아무리 ArticleItemWrapper에서 last-child로 속성을 적용하려 해도 되지 않는 것이었다.

그래서 기왕 섹션을 나눈김에, listSection을 추가해주었다. 그러자 다시 last-child가 정상적으로 동작하는 것을 확인할 수 있었다.

정상동작 코드

```tsx
const Wrapper = styled.div``;
const ListSection = styled.div``;
const ArticleItemWrapper = styled.div`
  margin-bottom: 12px;
  &:last-child {
    margin-bottom: 0;
    border: 1px solid red;
  }
`;
const LoadMoreSection = styled.div`
  margin-top: 40px;
`;

return (
    <Wrapper>
      <ListSection>
        {notificationList.map((item) => (
          <ArticleItemWrapper key={item.id}>
            <NotificationListItem item={item} handleClick={handleClick} />
          </ArticleItemWrapper>
        ))}
      </ListSection>
      <LoadMoreSection>
        <Button
          onClick={handleLoadMore}
          label="목록 더보기"
          variant="gray"
          size="md"
          width="240px"
          isLoading={vm.isLoading}
        />
      </LoadMoreSection>
    </Wrapper>
  );
```

*추가

물론, 서로 다른 element에다가 margin을 적용한거라, marginbottom + margintop이 동시적용이 아니라,

**마진 상쇄(Margin-collapsing)**에 의해 margin이 40만 적용되는 부분이 있기에 굳이 안 건드려도 될 수는 있다.

그러나 list Item의 마지막에 어떠한 다른 속성들이 추가가 될 수도 있는 거고,

어쨌든 마진 상쇄가 일어난다고는 해도 내가 css에 정의한 속성이 전혀 동작하지 않는 코드가 되는 것은 별개의 문제라고 판단하였기에, ListSection으로 감싸서 last-child가 의도대로 동작할 수 있도록 처리를 하였다.


참고

[https://developer.mozilla.org/en-US/docs/Web/CSS/:last-child](https://developer.mozilla.org/en-US/docs/Web/CSS/:last-child)