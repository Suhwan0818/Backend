# Node js 에 OpenCV 적용하기

https://www.npmjs.com/package/opencv4nodejs 내용을 참조합시다

## Node js 개념

핵심 개념

- 구글 v8 자바스크립트 엔진
- 고성능 네트워크 서버
- 단일 쓰레드, 이벤트 루트 기반
- 비동기 I/O처리
- 자바스크립트 기반
- 개발 생산성 향상
- 방대한 모듈 제공(npm)

간단한 로직을 기반으로 대용량(동시에 여러 request를 처리가 가능하다)의 특징을 가진다. 비동기 방식의 서비스에 가장 어울리는 서버 개요

해커톤 주제인 실시간 영상 처리 서비스는 네트워크 스트리밍 서비스에 가깝다. 빠른 응답이 가능한 비동기 방식을 사용하여 실시간 스트리밍에 문제가 없게 만들기 위하여 NodeJS를 선택

## Node.js 활용 용도

기본 프론트 단에서는 사진 파일 혹은 비디오만 보낼 것이다. 그것을 node 서버가 받아서 openCV처리 후 결과 값 만을 후속처리해서 프론트 단에 보낼 것이다. 아래 사진은 Application/Scripts 부분을 서버 Android 부분을 프론트라고 가정했을때 대략적인 백엔드 서버의 구현이다.

![node_server](/img/node_server.png)

기본 모든 처리를 백엔드 단에서 완료를 하고 스마트 폰이 아닌 외부 기기를 이용하는 가정을 하고 작성을 하였다. 처리 값을 OpenCV로 전부 처리하는 것이 아닌 부족한 부분 혹은 추가적인 기능이 생길 경우를 대비해 Tensorflow단을 추가하여 기능에 대한 다양성을 감안해두었다.

## Node js에 OpenCV 설정하기 기본

node gyp를 우선적으로 설치해준다 terminal 환경에 다음과 같이 타이핑한다.

```
> npm install node-gyp
```

여기서 gyp는 Node.js용 네이티브 애드온을 컴파일링하기 위한 크로스 플랫폼 커멘드라인 툴이다. 현재 팀의 프로젝트 내용이 **android**를 **kotlin**기반으로 제작하고 **OpenCV**의 경우 **python**혹은 **C++**를 이용하게 된다. 이때 가장 문제가 되는 것은 호환성이다. Node JS의 경우 Javascript를 기반으로 하기 때문 크로스 플랫폼의 기능이 필요로 하게된다.

> gyp의 기본 개념자체는 python을 기초로 한다. 만약 설치에 오류가 발생한다면 python을 설치하는 것으로 해결할 수 있다.

환경은 Docker를 추천하지만 기본 체제인 Window를 기본으로 설명하겠다. 다양한 플랫폼에서의 개발을 생각한다면 Docker를 쓰는 것이 옳은 판단이다.

```
> npm install --global windows-build-tools
```

마지막으로 OpenCV 셋팅을 다운로드 해준다.

```
> set OPENCV4NODEJS_DISABLE_AUTOBUILD=1
```

윈도우 환경에서 빌드 툴과 openCV설정을 완료했지만 아직 완성된것은 아니다. window툴을 일괄 설치하는 과정에서 필요없는 툴도 다운로드 되었고 필요한 셋팅만을 꺼내쓸수 없기 때문에 **chocolatey**라는 라이브러리를 통해서 윈도우 툴을 관리 및 설정할 것이다.  
[설치하기](https://chocolatey.org/install)

```
> choco -v
```

위 명령어를 통해 버전이 출력된다면 성공적으로 다운받은 것이다. 이중 OpenCV만을 사용하기 위한 툴만을 다운로드 하겠다.

```
> choco install OpenCV -y -version 4.1.0
```

OpenCV를 위한 툴이 모두 다운로드 되면 환경변수를 설정해주어야 한다.

1. 시스템 환경 변수의 Path 환경 변수에 %**OPENCV_BIN_DIR**%를 추가한다.
2. OPENCV_INCLUDE_DIR 라는 환경변수를 새로 생성 후 경로는 따로 건드리지 않았다면 **C:\tools\opencv\build\include**가 디폴트 값이다.
3. OPENCV_LIB_DIR 라는 환경변수를 새로 생성 후 값은 **C:\tools\opencv\build\x64\vc15\lib**
4. OPENCV_BIN_DIR 라는 환경변수를 새로 생성 후 값은 **C:\tools\opencv\build\bin**

설정을 완료한 것들을 저장해준다.

```
> npm install --save opencv4nodejs
```

전부 완료했다면 node.js 폴더에 있는 package.json 파일에 아래와 같은 형식으로 코드를 추가해준다.

```js
{
  "name": "my-project",
  "version": "0.0.0",
  "dependencies": {
    "opencv4nodejs": "^X.X.X"
  },
  "opencv4nodejs": {
    "disableAutoBuild": 1,
    "opencvIncludeDir": "C:\\tools\\opencv\\build\\include",
    "opencvLibDir": "C:\\tools\\opencv\\build\\x64\\vc14\\lib",
    "opencvBinDir": "C:\\tools\\opencv\\build\\x64\\vc14\\bin"
  }
}
```

OpenCV값을 끌어다 쓰는 개념은 다 다르지만 기본 아래와 같은 코드로 변수에 넣어 사용한다.

```js
const cv = require('opencv4nodejs')
```

## 서버의 구체적인 구현

세부적인 구현은 DB 개발과 협업을 할 예정이며 DB에 대한 예상도는 아래 사진과 같은 구현이 예상된다.

![DB_structure](/img/DB_structure.png)

프로젝트 구현 단계에서 값들은 충분히 바뀔 수 있으며 기본적인 구현은 위 사진을 따를 예정이다. 이 밖에 추가할 정보는 기능적인 구현에 대한 대략적인 예시이다. 아래 그림을 참조하겠다.

![function_base](/img/function_base.png)

가장 기본적인 기능만 구현했으며 위 사진의 기능을 무조건 구현하되 추가할 수 있는 구현을 추가할 예정이다.
