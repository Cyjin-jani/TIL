## react-hook-form onSubmit 새로고침 안되도록 하기
<br>

보통 form을 submit하게 되면, 페이지를 새롭게 새로고침을 한다.

하지만, 만약 페이지 안에 특정 섹션 안에서만 form submit이 일어나고,
해당 섹션의 컴포넌트만 re-render가 되어야 한다면,
기존의 react-hook-form에 onSubmit를 submit의 트리거로 주면 안된다.

조금은 편법일 수도? 있지만,

form submit을 하는 버튼을 type button으로 하고, onClick을 부여해서 새롭게 submit 함수를 정의해주면 된다.

해당 코드는 필터 검색을 통해 새로 리스트 데이터를 부르는 로직이다.

```tsx
const form = useForm({
  resolver: classValidatorResolver(FindDto),
});

const handleSubmit = async (
  e: React.MouseEvent<HTMLButtonElement, MouseEvent>,
  data: any
) => {
  const filterData: FindDto = {
    code: data.code,
    phoneNumber: data.phoneNumber,
    companyName: data.companyName,
    offset: 0,
    limit: 4,
  };

  const queryDto: FindDto = await deserialize(FindDto, {
    filterData,
  });

  await vm.getList(queryDto);
};

///...... 렌더링 영역
<FormProvider form={form} onSubmit={handleSubmit}>
  <FilterSection>
    <ItemBox>
      <Input
        name="code"
        variantSize="sm"
        label="아이디"
        placeholder="아이디입력"
        width="100%"
      />
    </ItemBox>
    <ItemBox>
      <Input
        name="phoneNumber"
        variantSize="sm"
        label="휴대폰번호"
        placeholder="-를 빼고 입력"
        width="100%"
      />
    </ItemBox>
    <ItemBox>
      <Input
        name="companyName"
        variantSize="sm"
        label="회사명"
        placeholder="회사명입력"
        width="100%"
      />
    </ItemBox>

    <ButtonBox>
      <Button
        type="button"
        variant="black"
        size="sm"
        width="98px"
        label="고객사 검색"
        onClick={(e) => handleSubmit(e, form.getValues())}
      />
    </ButtonBox>
  </FilterSection>
</FormProvider>
```

위와 같이 하면, 

검색 버튼은 submit의 역할은 하지만, 실제로 페이지가 새로고침 되는 re-render 트리거를 일으키지 않기 때문에, 새로고침 없이 새롭게 데이터를 요청받아 뿌려주는 것이 가능하다.

위 로직이 없다면,

분명 getList는 비동기 요청이나, submit이 되므로 페이지가 새로고침 되는 불편함이 있어, 비동기 요청임에도 비동기 같지 않은 동작이 된다.

때문에 이러한 온클릭 이벤트는 조심해야 하는 것 같다.