# Babel

`"Babel is a Javascript compiler"` 공식 사이트 가장 전면에 있는 문구이다.  

바벨의 역할은 ECMAScript 2015 이상의 코드를 이전의 버전에서도 문제없이 실행될 수 있게 만들어주는 툴 모듬입니다. 주로 아래와 같은 이유로 사용된다.  

- 구문 변환  
- 자원하지 않는 이전 브라우저에서 최신 기능을 제공하는데 필요한 코드 제공  
- 소스 코드 변환  

예를 들어 코드를 보면
```js
//내가 바벨에 넣은 함수(화살표 함수 형태)
[1, 2, 3].map(n=>n+1);
//바벨에서 실제 출력해주는 문구(화살표 함수가 없어 변환을 진행)
[1, 2, 3].map(function(n){
    return n + 1;
})
```
위와 같이 컴파일러 기능을 수행한다.  

현대에 사용하는 **ECMAScript 2015+** 이상 버전 역시 지원한다. 위 예시와 같이 새로운 자바 스크립트 기능이 나왔을 때도 브라우저의 지원을 기다릴 필요없이 바로 적용이 가능하다.  

### React에서의 사용법  
1. 패키지 설치
```s
npm install --save-dev @babel/core @babel/cli @babel/preset-env  
```  
2. 루트 폴더에 `babel.config.json` 파일 생성
```json
{
    "presets":[
        [
            "@babel/preset-env",
            {
                "target":{
                    "edge": "17",
                    "firefox": "60",
                    "chrome": "67",
                    "safari": "11.1"
                },
                "useBuiltIns": "usage",
                "corejs": "3.6.5"
            }
        ]
    ]
}
```
> 위 브라우저 리스트는 임의로 정해졌다. 기능과 역할에 맞게 프로젝트 별로 조정해 줄 필요가 있다.  

현재까지 상황으로 `src` 파일을 컴파일해서 `lib` 안으로 넣어줍니다.  

```s
./node_modules/.bin/babel src --out-dir lib
```  
능
여기까지 진행했다면 기본적인 설정은 끝이다. 이제 설치한 각각의 설정들을 보겠다.  

처음 `npm`을 통해 패키지를 각각 다른 종류를 설치해주었다. Babel 7 버전 이후로 위와 같이 여러 종류의 구분으로 나뉘었다.  

### 기능
`@Babel/core` 패키지를 성공적으로 설치했다면 다음과 같은 설계가 가능하다.  

```js
const babel = require('@babel/core');

babel.transformSync('code', optionsObject);
```  
바벨을 사용하기 위한 가장 기본이되는 패키지로써 자세한 사항은 [Babel 공식 문서](https://babeljs.io/docs/en/babel-core)를 통해 확인할 수 있다. ([@bable/cli](https://babeljs.io/docs/en/babel-cli) 역시 마찬가지다.)  