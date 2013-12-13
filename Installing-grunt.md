이 문서는 특정 버전의 Grunt와 Grunt plugin 설치하는 방법에 대해 다룬다. 하지만 [[Getting Started]] 문서를 먼저 보고 봐야 한다.

## Overview
Grunt와 Grunt plugin은 [package.json](https://npmjs.org/doc/json.html)의 [devDependencies](https://npmjs.org/doc/json.html#devDependencies)에 정의되어 있어야 한다. 그래야 `npm install` 컴멘트 명령어 한방으로 프로젝트와 관련된 의존성 모듈 모두를 설치할 수 있다.  Grunt 위키 [첫 페이지](https://github.com/gruntjs/grunt/wiki/)에서 Grunt의 최신 안정 버전과 개발버전을 확인할 수 있다.

## Installing a specific version
특정 버전의 Grunt나 Grunt plugin이 필요하다면, `npm install grunt@VERSION --save-dev`를 실행한다. 이때 `VERSION`에는 여러분이 필요한 버전을 넣어준다. 그러면 그 버전을 설치하고 package.json의 devDependencies에 이를 추가한다.

`npm install`과 `--save-dev` 옵션을 같이 쓰면 `package.json`에서 설치된 모듈의 버전을 지정할 때 [tilde version range]이 사용된다. 이는 장점이 많다. [semver]단위로 개발이 지속되기 때문에 지정한 버전에 새로운 패치가 릴리즈 되면 자동으로 업그레이드 한다.

[tilde version range]: https://npmjs.org/doc/json.html#Tilde-Version-Ranges
[semver]: http://semver.org

## Installing a published development version
배포된 개발 버전도 설치할 수 있다. 신규 기능이 계속 개발되고 있기 때문에, 빌드된 Grunt가 정기적으로  npm에 배포된다. 이 빌드 판은 명시적으로 버전을 지정하지 않고는 설치할 수 없다.그래서 보통은 빌드 넘버나 alpha/beta/release 같은 명칭을 갖고 있다.

Grunt 특정 버전 설치과정과 동일하게 `npm install grunt@VERSION --save-dev`를 필요한 `VERSION`을 넣어서 실하면 npm이 프로젝트 폴더에 그 버전의 Grunt를 설치하고`package.json`의 devDependencies에 모듈 버전을 추가한다.

한가지, 여러분이 지정한 버전과 관계없는 [tilde version range][]가 `package.json`에 설정된다. **이건 별로 좋지 않다.** 지정한 개발 버전에 신규 패치가 릴리즈 되었는데 이 패치가 여러분의 빌드를 깨는 코드라해도 npm을 통해서 설치될 수 있기 때문이다.

_이런 경우, 수동으로 `package.json`의 devDependencies에서 ~(tilde)를 **꼭 제거해야 한다.** 그러면 그 특정 개발 버전에만 한정된다._

이 과정은 Grutn plugin에서도 동일하다.

## Installing directly from github
미배포 버전의 Grunt나 Grunt plugin의 위험을 감수할 생각이 있다면,
[dependency로 git URL](https://npmjs.org/doc/json.html#Git-URLs-as-Dependencies)을 지정하는 방법을 익혀라. 그리고 `commit-ish`로 실제 커밋 SHA(branch 명이 아님)를 지정 한다. 이러면 항상 버전을 꼭 찝은 grunt를 보장받을 수 있다.

git URL은 공식 Grunt 레파지토리나 포크받은 레파지토리의 URL이 된다.