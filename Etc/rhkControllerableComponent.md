## react-hook-form에서 Controller 활용하여 form 핸들링하기 (onChange, value 등)
<br>

react hook form 을 제대로 활용하기 전에는 

textinput 과 같은 기본적으로 value, onChange(onTextChange) 등의 props를 제공하지 않는

컴포넌트의 경우 (ex. TouchableOpacity) 어떤 식으로 form에 연결해야 할지 알 지 못했다.

그랬기에 아래와 같은 기이한(?) 코드의 탄생이...

```tsx
import React, {useState} from 'react';

import styled from 'styled-components/native';
import {
  useFormContext,
  Controller,
  FieldValues,
  Path,
  UnpackNestedValue,
  PathValue,
} from 'react-hook-form';

export type Props<T extends FieldValues> = {
  id: Path<T>;
  options: {
    label: string;
    value: string;
    svgIcon: SVGElement;
    svgIconChecked: SVGElement;
  }[];
  defaultValues?: string[];
};

const Atom = <T extends FieldValues>({
  id,
  options,
  defaultValues,
}: Props<T>) => {
  const methods = useFormContext<T>();

  const [isChecked, setIsChecked] = useState<string[]>(defaultValues || []);

  const handleChecked = (val: string) => {
    const valueIndex = isChecked.indexOf(val);
    if (valueIndex > -1) {
      const newArr = isChecked.filter((value, index) => {
        if (valueIndex !== index) {
          return value;
        }
      });
      setIsChecked(newArr);
      methods.setValue(id, newArr as UnpackNestedValue<PathValue<T, Path<T>>>);
    } else {
      setIsChecked(prev => [...prev, val]);
      methods.setValue(id, [...isChecked, val] as UnpackNestedValue<
        PathValue<T, Path<T>>
      >);
    }
  };

  return (
    <Controller<T>
      control={methods.control}
      name={id}
      defaultValue={defaultValues}
      render={() => {
        return (
          <CheckBoxView>
            {options.map(opt => (
              <TouchableNoFB
                onPress={() => handleChecked(opt.value)}
                key={opt.value}>
                <CheckBox checked={isChecked.includes(opt.value)}>
                  {isChecked.includes(opt.value)
                    ? opt.svgIconChecked
                    : opt.svgIcon}

                  <CaptionLabel checked={isChecked.includes(opt.value)}>
                    {opt.label}
                  </CaptionLabel>
                </CheckBox>
              </TouchableNoFB>
            ))}
          </CheckBoxView>
        );
      }}
    />
  );
};

const CheckBoxView = styled.View`
  flex-direction: row;
  flex-wrap: wrap;
  justify-content: space-between;
  align-items: center;
`;

const CaptionLabel = styled.Text<{checked: boolean}>`
  margin-top: 4px;
  font-size: 12px;
  font-weight: 400;
  color: ${({theme}) => theme.colors.text.paragraph};

  ${({checked, theme}) =>
    checked &&
    `
      color: ${theme.colors.secondary.main};
    `};
`;

const CheckBox = styled.View<{checked: boolean}>`
  width: 76px;
  height: 76px;
  margin-top: 7.5px;
  margin-bottom: 7.5px;
  border-width: 1px;
  border-radius: 18px;
  border-color: ${({theme}) => theme.colors.others.inputBorder};
  flex-direction: column;
  justify-content: center;
  align-items: center;
  background-color: ${({theme}) => theme.colors.background.default};
  ${({checked, theme}) =>
    checked &&
    `
      border-color: ${theme.colors.secondary.main};
      background-color: ${theme.colors.secondary.light};
    `};
`;

const TouchableNoFB = styled.TouchableWithoutFeedback``;

export default Atom;
```

위에서 결국 state를 활용하여 값의 상태 변화를 감지/저장하고,

form 자체에 value를 업데이트 해주기 위해 setValue를 해주었다

하지만 이는 form 자체에서 컨트롤하고 있는 isValid 등을 활용할 수 없었다. (순수하게 value 만 업데이트 되었고, 폼 라이브러리를 쓰는 중대한 이유 중 하나인 유효성 검사를 활용할 수 없었던 것...)

따라서 위 코드는 천지개벽수준으로 뜯어고치는 게 필요했고, 기본적으로 Controller에서 제공하는 field인 value, onChange를 활용하여 form 의 value를 핸들링 할 수 있게 하였다.

```tsx

import React from 'react';
import {
  useFormContext,
  Controller,
  FieldValues,
  Path,
  PathValue,
} from 'react-hook-form';
import {Dimensions} from 'react-native';
import styled from 'styled-components/native';

export type Props<T extends FieldValues> = {
  id: Path<T>;
  option: {
    label: string;
    value: string;
    svgIcon: SVGElement;
    svgIconChecked: SVGElement;
  };
  defaultValue?: boolean;
};

const Atom = <T extends FieldValues>({id, option, defaultValue}: Props<T>) => {
  const methods = useFormContext<T>();
  return (
    <Controller<T>
      control={methods.control}
      name={id}
      defaultValue={defaultValue as PathValue<T, Path<T>>}
      render={({field: {onChange, value}}) => {
        const isChecked = !!value;
        return (
          <TouchableNoFB
            onPress={() => {
              onChange(!value);
            }}>
            <CheckBox checked={isChecked}>
              {isChecked ? option.svgIconChecked : option.svgIcon}

              <CaptionLabel checked={isChecked}>{option.label}</CaptionLabel>
            </CheckBox>
          </TouchableNoFB>
        );
      }}
    />
  );
};

---중략----

export default Atom;
```

왜 때문인지 모르게, (아마 아래와 같은 식으로 표시가 되어있었기에)

```tsx
onChange: (...event: any[]) => void;
```

onChange에 파라미터로 event가 아닌 value를 넣어서 값을 바꿀 수 있다는 생각을 못했었고,

자연스레 onChange에 파라미터로 form의 value를 넘겨주어 

form의 value와 유효성 핸들링이 가능하게 되었다.