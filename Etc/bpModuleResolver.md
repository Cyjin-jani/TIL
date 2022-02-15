## 점점... 멈춰! babel plugin module resolver 사용하여 절대경로 사용하기
<br>

파일 디렉토리 구조 및 뎁스가 복잡하고 깊어질 수록 점점(..)은 더 멀어진다....
문제는 처음 만든 이후 디렉토리 구조에 변경이 일어날 경우, 상대경로를 하나하나 수정해주어야 하는 부분이다.
처음부터 절대경로를 사용하면 해결되는 부분이지만, 디렉토리 구조가 복잡할수록 손 아프게(?) 한땀 한땀 경로를 적어야 하는 노오오력이 필요해진다.

이를 손쉽게 해결할 수 있는게 바로 babel plugin module resolver다

사용하게 되면 아래와 같이 쉽게 from 절을 적을 수 있다.

```tsx
// Use this:
import MyUtilFn from 'utils/MyUtilFn';
// Instead of that:
import MyUtilFn from '../../../../utils/MyUtilFn';
```

[https://github.com/tleunen/babel-plugin-module-resolver](https://github.com/tleunen/babel-plugin-module-resolver)

회사에서 하는 설정으로는 아래와 같은 방법으로 babel.config.js에 정의하고 있다.

```tsx
[
        'module-resolver',
        {
          root: ['./'],
          alias: {
            '~presentation/': './src/presentation/',
            '~data/': './src/data/',
            '~domain/': './src/domain/',
            '~utils/': './src/utils/',
            '~helper/': './src/helper/',
            '~enum/': './src/enum/',
          },
        },
      ],
```