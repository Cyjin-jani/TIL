## JS 객체 배열 같은 속성 값 묶어서 렌더링하기
<br>

아래와 같은 배열이 있다고 가정.

```tsx
const stockList = [
{id: 'a123dw4', createdAt: '2022-02-22'}, 
{id: 'asdf', createdAt: '2022-02-22'}, 
{id: 'daw', createdAt: '2022-02-02'},
{id: 'zxc', createdAt: '2022-02-02'},
]
```

방법1

array.map안에서 로직 활용!

- 배열은 최신순으로 정렬되어 있다고 가정.
- 가장 최근의 date를 prevDate에 저장.
- 변경이 발생하면 prevDate를 업데이트 함과 동시에 headerRenderFlag를 true로 설정하여,
무슨 요일인지 날짜가 보여지도록 표시.

```tsx
const renderList = () => {
    let prevDate: Date;
    let headerRenderFlag;
    return (
      <ListSection>
        {stockList.map((item, idx) => {
          if (
            prevDate &&
            formattedDateAndDay(prevDate) ===
              formattedDateAndDay(item.createdDateTime)
          ) {
            headerRenderFlag = false;
          } else {
            prevDate = item.createdDateTime;
            headerRenderFlag = true;
          }

          return (
            <ListWrapper key={item.id}>
              {headerRenderFlag && (
                <DateLabel $hasMarginTop={idx === 0 ? false : true}>
                  {formattedDateAndDay(item.createdDateTime)}
                </DateLabel>
              )}
              <WareHousingListItem item={item} />
            </ListWrapper>
          );
        })}
      </ListSection>
    );
  };
```

방법2

js의 Map을 활용!

- 역시 배열은 최신순 날짜 기준으로 정렬되어 있다 가정.
- 배열의 객체를 순환하면서 객체의 속성 중 createdDateTime을 key로 선택.
- 해당 key안에 똑같은 key(createdDateTime)을 가진 객체 자체를 key에 해당하는 value로 Map에 저장
- 이후 Map을 순환가능한 object로 변경 (entries)후, 배열형태로 치환 (Array.from)
- map을 두번 돌면서 (큰 순환: 날짜 기준, 작은 순환: 해당 날짜 안의 데이터 전부 표시)  렌더링

```tsx
const renderList = () => {
    function groupBy(list: any[], keyGetter: any) {
      const map = new Map();
      list.forEach((item) => {
        const key = keyGetter(item);
        const collection = map.get(key);
        if (!collection) {
          map.set(key, [item]);
        } else {
          collection.push(item);
        }
      });
      return map;
    }

    const grouped = groupBy(stockList, (st: StockListModel) =>
      formattedDateAndDay(st.createdDateTime)
    );

    const groupedStockList = Array.from(grouped.entries());

    return (
      <>
        {groupedStockList.map((item) => (
          <ListContainer key={item[0]}>
            <DateLabel>{item[0]}</DateLabel>
            {item[1].map((it: StockListModel) => (
              <WareHousingListItem key={it.id} item={it} />
            ))}
          </ListContainer>
        ))}
      </>
    );
  };
```