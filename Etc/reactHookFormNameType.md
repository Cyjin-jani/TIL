## react-hook-form 커스텀 컴포넌트 name 타입 문제
<br>
react-hook-form을 사용하여 컴포넌트를 구성하는 경우, 
<br>
name props에 string 타입을 사용하곤 하였으나,
<br>
아래와 같은 문제가 나타나곤 했었다. (ts2322)

```tsx
(JSX attribute) name: `${string}` | `${string}.${string}` | `${string}.${number}`
'string' 형식은 '`${string}` | `${string}.${string}` | `${string}.${number}`' 형식에 할당할 수 없습니다.ts(2322)
controller.d.ts(18, 5): 필요한 형식은 여기에서 'IntrinsicAttributes & { render: ({ field, fieldState, formState, }: { field: ControllerRenderProps<Record<string, any>, `${string}` | `${string}.${string}` | `${string}.${number}`>; fieldState: ControllerFieldState<...>; formState: FormState<...>; }) => ReactElement<...>; } & UseControllerProps<...>' 형식에 선언된 'name' 속성에서 가져옵니다.
```

이를 회피하고자, 

name as `${string}`
이런식으로 문제를 넘겼었는데, 공식문서를 살펴보니 해결책이 있었다.

바로 Path라는 녀석을 활용하는 것이다.

공식문서에서도 Path를 이용하면 커스텀 컴포넌트의 name 이라는 prop의 타입에 유용하다고 소개하고 있다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/9fc6b211-e9e1-41a3-9167-1b429fd73db7/Untitled.png)

실제 코드 예시는 아래와 같다. (mui 사용한 체크박스)

```tsx
export type Props<T extends FieldValues> = {
  name: Path<T>;
  label?: string;
  defaultValue?: PathValue<T, Path<T>>;
  onClick?: (event: React.MouseEvent<HTMLButtonElement, MouseEvent>) => void;
  labelVariant?: Variant;
} & AtomCheckboxProps;

const Molecules = <T extends FieldValues>({
  name,
  label,
  defaultValue,
  colorOpt,
  labelVariant = 'button2',
  ...rest
}: Props<T>) => {
  const { control } = useFormContext<T>();
  const dv = get(
    control._defaultValues,
    name,
    typeof defaultValue !== 'undefined' ? !!defaultValue : false
  );

  return (
    <FormControl component="fieldset">
      <Controller<T>
        control={control}
        name={name}
        render={({ field: { onChange, value } }) => {
          return (
            <MuiFormCtrlLabel
              value={!!value}
              control={
                <Checkbox
                  onChange={onChange}
                  colorOpt={colorOpt}
                  value={!!value}
                  checked={!!value}
                  {...rest}
                />
              }
              label={
                label && (
                  <CheckBoxLabel variant={labelVariant}>{label}</CheckBoxLabel>
                )
              }
            />
          );
        }}
        defaultValue={dv}
      />
    </FormControl>
  );
};

export default Molecules;

const CheckBoxLabel = styled(Typography).attrs({
  component: 'span',
})`
  font-family: 'Noto Sans KR', 'Helvetica Neue', Helvetica, Arial, sans-serif;
  margin-left: 8px;
`;

const MuiFormCtrlLabel = styled(FormControlLabel)`
  && {
    margin: 0;
  }
  & > span {
    padding: 0;
  }
  & > span > input {
    padding: 0;
  }
  & > span .MuiTouchRipple-root {
    display: none;
  }
  & > span:hover {
    background: transparent;
  }
  .Mui-checked:hover {
    background: transparent;
  }
`;
```