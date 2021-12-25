# eslint란?  

개발자 개발에 있어 코드에 다양성은 무궁무진하다. 이것을 통합 시키기란 쉬운일이 아니다. 특히 협업에 있어서는 다양성을 고려해준다쳐도 나중에 합치는 일이 있을때 너무 어렵다는 점이 있다. 이런 일을 대비하기 위해 우리는 **Eslint**를 사용한다.  

예를 들어 아래 코드를 보자

```c
int a;
int b;

int numForAdd;
int numForMinus;
```

위 코드를 봤을때 a와 b의 경우 어떻게 사용할지 전혀 감이 오지 않는다. 하지만 numForAdd와 numForMinus 같은 경우 누가봐도 더하기와 빼기를 위한 변수라는 것을 알 수 있다. 이렇게 **누구나 알아볼 수 있게 만드는 것이 목적**이다.  

## react에 적용하기  
react가 설치된 가정 하에 airbnb 기준을 설치한다

```
yarn add --dev eslint-config-airbnb prettier eslint-config-prettier eslint-plugin-prettier prettier eslint-plugin-prettier eslint-config-prettier eslint-config-airbnb-base eslint-plugin-import
```

airbnb는 여러분이 아는 그 회사가 맞다. 이 회사의 디자인 패턴이 가장 협업에 적합하다는 평가를 받아 사용하게 되었다. 다음은 react 매인 폴더에 prettier.js라는 파일을 만들어줄 것이다.  

```js
//prettier.js
module.exports = {
  arrowParens: 'avoid', // 화살표 함수 괄호 사용 방식
  bracketSpacing: false, // 객체 리터럴에서 괄호에 공백 삽입 여부
  endOfLine: 'auto', // EoF 방식, OS별로 처리 방식이 다름
  htmlWhitespaceSensitivity: 'css', // HTML 공백 감도 설정
  jsxBracketSameLine: false, // JSX의 마지막 `>`를 다음 줄로 내릴지 여부
  jsxSingleQuote: false, // JSX에 singe 쿼테이션 사용 여부
  printWidth: 80, //  줄 바꿈 할 폭 길이
  proseWrap: 'preserve', // markdown 텍스트의 줄바꿈 방식 (v1.8.2)
  quoteProps: 'as-needed', // 객체 속성에 쿼테이션 적용 방식
  semi: true, // 세미콜론 사용 여부
  singleQuote: true, // single 쿼테이션 사용 여부
  tabWidth: 2, // 탭 너비
  trailingComma: 'all', // 여러 줄을 사용할 때, 후행 콤마 사용 방식
  useTabs: false, // 탭 사용 여부
  parser: '', // 사용할 parser를 지정, 자동으로 지정됨
  filepath: '', // parser를 유추할 수 있는 파일을 지정
  rangeStart: 0, // 포맷팅을 부분 적용할 파일의 시작 라인 지정
  rangeEnd: Infinity, // 포맷팅 부분 적용할 파일의 끝 라인 지정,
  requirePragma: false, // 파일 상단에 미리 정의된 주석을 작성하고 Pragma로 포맷팅 사용 여부 지정 (v1.8.0)
  insertPragma: false, // 미리 정의된 @format marker의 사용 여부 (v1.8.0)
  overrides: [
    {
      files: '*.json',
      options: {
        printWidth: 200,
      },
    },
  ], // 특정 파일별로 옵션을 다르게 지정함, ESLint 방식 사용
};

```  
이 파일의 경우 react를 사용할때 코드를 예쁘게 처리를 하는 것이다. 자동완성 등 기능이 들어있고 코드를 분석해보는게 공부에 좋다.  

다음 eslint 설정을 해준다  eslint.js를 메인에 설치해준다.  

```js
//eslint.js
module.exports = {
  env: {
    browser: true,
    es2021: true,
  },
  extends: [
    'eslint:recommended',
    'plugin:react/recommended',
    'airbnb-base',
    'plugin:prettier/recommended',
  ],
  parserOptions: {
    ecmaFeatures: {
      jsx: true,
    },
    ecmaVersion: 12,
    sourceType: 'module',
  },
  plugins: ['react', 'prettier'],
  rules: {
    'react/react-in-jsx-scope': 'off',
    // allow jsx syntax in js files (for next.js project)
    'react/jsx-filename-extension': [1, {extensions: ['.js', '.jsx']}],
  },
};
```  

```yarn upgrade -R eslint```