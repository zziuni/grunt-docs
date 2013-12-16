Grunt와 Grunt 플러그인의 설치와 관리는 [npm](https://npmjs.org/) 을 통해서 한다. npm은 [Node.js](http://nodejs.org/)의 패키지 메니징 도구다.

_Grunt 0.4.x를 사용하려면 Node.js 버전이 `>=0.8.0` 이여야 한다. Node.js에서 홀수 버전은 안정화되지 않은 개발 버전이다._

## Installing the CLI

**Grunt 0.3을 이미 사용하고 있고 0.4.x로 업그레이드하는 거라면, [Grunt 0.3 Notes](upgrading-from-0.3-to-0.4#grunt-0.3-notes) 문서를 먼저 보자.**

Grunt를 사용하려면 먼저 콘솔 어디서나 실행할 수 있는 Grunt's Command line interface (CLI)를 설치해야 한다. 이때 OSX나 *nix, BSD에서는 sudo가, 윈도우즈에서는 administrator 권한이 필요할 수도 있다.


```shell
npm install -g grunt-cli
```

`grunt-cli`를 설치하면, 여러분의 시스템 경로에 `grunt`란 컴멘드가 자동으로 추가해서, 어느 디렉터리에서나 `grunt`를 사용할 수 있게 된다.

그러나 `grunt-cli`가 Grunt task runner(즉, `grunt`)를 설치하지는 않는다. Grunt CLI의 역할은 그저 `Gruntflie`라는 파일이 있는 위치에 설치된 Grunt를 찾아서 실행하는 것이다. 그러므로, 한 장비에서 여러 버전의 Grunt를 설치할 수 있다.


## How the CLI works

`grunt` 컴멘트를 치면, grunt-cli가 node의 `require()`를 사용해서 프로젝트 로컬의 grunt를 실행한다. 그러므로 해당 프로젝트의 루트 폴더가 아니여도 하위 폴더 어디서든 `grunt`를 실행할 수 있다.

특정 위치에 설치된 Grunt를 찾은 후에 CLI는 Grunt 라이브러리의 로컬 설치 본을 불러온다. 이때  필요한 환경을 설정하기 위해서 `Gruntfile`란 파일을 사용한다. 그리고 CLI는 그 안에 어떤 동작을 위해 설정한 task들을 실행한다.

*이 과정이 더 궁금하면 [코드](https://github.com/gruntjs/grunt-cli/blob/master/bin/grunt)를 읽어보자. 겁나 짧다.*

## Working with an existing Grunt project
만약 이미 Grunt CLI가 설치되어있고, 해당 프로젝트에 `package.json`과 `Grunfile`를 설정했다면, 한결 쉽게 작업할 수 있다.

1. 프로젝트 루트 폴더로 이동한다.
1. `npm install`로 프로젝트가 참조하는 모듈들을 설치한다.
1. `grunt`를 실행한다.

이게 전부다. 사용 가능한 task는 `gurnt --help` 컴멘드로 볼 수 있지만 보통 해당 프로젝트 문서를 참고한다.

## Preparing a new Grunt project
프로젝트를 새로 시작할 때는 보통 `package.json`과 `Grunfile`라는 파일이  먼저 있어야 한다.

**package.json**: 이 파일은 해당 프로젝트를 npm 모듈로 [npm]에 퍼블리싱할 때 사용하는 메타데이터 저장 파일이다. 프로젝트에 필요한 grunt와 grunt 플러그인들은 이 파일의 [devDependencies]라는 항목에 나열한다.

**Gruntfile**: 이 파일의 이름은 `Gruntfile.js`이거나 `Gruntfile.coffee`이다. task를 설정하거나 정의하고 Grunt 플러그인을 불러오는데 사용한다.

## package.json

`package.json`파일은 `Gruntfile`과 함께 프로젝트 루트 디렉토리에 있어야 하고, 프로젝트 소스와 함께 커밋(commit)되야 한다. `package.json`이 있는 폴더에서 컴맨드 명령어 `npm install`를 실행하면 이 파일 안에 있는 dependency 목록의 모듈들을 해당 버전으로 인스톨한다.

프로젝트에 `package.json`를 추가하는 방법은 여러가지가 있다.

* 대부분의 [grunt-init] 템플릿은 자동으로 프로젝트 전용  `package.json`파일을 생성한다.
* [npm init] 컴맨드 명령어는 기본 `package.json`을 생성한다.
* 다음 예제를 기초로 필요한 부분은 [specification][json] 문서를 참고 해서 확장한다.

```js
{
  "name": "my-project-name",
  "version": "0.1.0",
  "devDependencies": {
    "grunt": "~0.4.2",
    "grunt-contrib-jshint": "~0.6.3",
    "grunt-contrib-nodeunit": "~0.2.0",
    "grunt-contrib-uglify": "~0.2.2"
  }
}
```

### Installing Grunt and gruntplugins
Grunt와 플러그인을 설치와 동시에 `package.json`와 연동시키려면 `npm install <module> --save-dev` 컴맨드 명령어를 사용한다. 이러면 `<module>`만 설치하고 끝나지 않고, 자동으로 `package.json`의 [devDependencies] 항목에 추가된다. 버전은 [tilde version range]를 사용한다.

예를 들면, 다음 컴맨드 명령어는 프로젝트에 Grunt 최신버전을 설치하고 `package.json`의 devDependencies 항목에 grunt를 추가한다.

```shell
npm install grunt --save-dev
```

grunt 플러그인과 다른 node 모듈도 마찬가지다. 이렇게 설치하면 프로젝트의 `package.json`이 갱신된다.

## The Gruntfile
`Gruntfile.js`나 `Gruntfile.coffee`파일은 `package.json`와 함께 프로젝트 루트 폴더에 있어야 하는 자바스크립트 혹은, 커피스크립트 파일이다. 그리고 이 파일은 프로젝트 소스의 일부로 같이 커밋되야 한다.

다음은 `Gruntfile`의 내부 구성요소다.

* "wrapper" 함수.
* 프로젝트와 task의 환경설정.
* grunt plugin과 task 로딩.
* 사용자 정의 task

### An example Gruntfile
다음 `Gruntfile`은 프로젝트의 메타데이터를 `package.json`에서 가져와서 Grunt config로 주입한다. 그리고 [grunt-contrib-uglify] 플러그인의 `uglify` task을 사용해서 소스코드를 미니파이(minify)하도록 설정하고, 메타데이터를 이용해서 동적인 베너 주석도 생성한다. 그리고 `uglify` task를 컴맨드 라인에서 `grunt` 명령어를 실행할 때 실행되는 기본 task로 지정했다.

```js
module.exports = function(grunt) {

  // 프로젝트 환경설정.
  grunt.initConfig({
    pkg: grunt.file.readJSON('package.json'),
    uglify: {
      options: {
        banner: '/*! <%= pkg.name %> <%= grunt.template.today("yyyy-mm-dd") %> */\n'
      },
      build: {
        src: 'src/<%= pkg.name %>.js',
        dest: 'build/<%= pkg.name %>.min.js'
      }
    }
  });

  // "uglify" task를 지원하는 플러그인 로드.
  grunt.loadNpmTasks('grunt-contrib-uglify');

  // Default task(s).
  grunt.registerTask('default', ['uglify']);

};
```

이 코드가 우리가 볼 `Grunfile` 예제 코드의 전다. 파트별로 살펴보자.

### The "wrapper" function
모든 `Gruntfile`(그리고 그 플러그인)은 래퍼(wrapper) 함수를 기본 형태로 사용한다. 모든 Grunt 코드는 이 함수 안쪽에 있어야 한다.

```js
module.exports = function(grunt) {
  // grunt 관련 코드는 여기에
};
```

### Project and task configuration
대부분의 Grunt task는 configuration을 위한 데이터로 [[grunt.initConfig|grunt#grunt.initconfig]]  메서드의 인자로 전달되는 객체를 사용한다.(이를 configuraton 객체라 하자.)

위의 예제에서, `grunt.file.readJSON('package.json')` 코드는 grunt config로 `package.json`에 저장된 JOSN 메타데이터를 인포트한다. 그러면 `<% %>` 템플릿 문자열을 사용해서 config의 모든 프로퍼티를 참조할 수 있다.(`package.json`값을 불러올 수 있다는 말이다.) 파일패스나 파일 목록 같은 configuration 데이터는 반복을 줄이기 위해서 이 방법으로 지정한다.

여러분의 task가 필요로하는 프로퍼티와 충돌하지 않는 이상 configuraton 객체안에는 어떤 값을 넣어도 상관없다. 그리고 이 파일은 단순 JSON이 아닌 자바스크립트 파일이므로, 유효한 JS코드는 모두 사용가능하다. 즉, 필요한 configuration을 동적으로 생성할 수도 있다.

다른 task도 마찬가지지만, [grunt-contrib-uglify] 플러그인의 `uglify` task는 configuration 객체에서 동명의 프로퍼티 명(uglify)으로 설정에 필요한 값을 찾는다. 여기서는 옵션값인 `banner`와 소스파일(src) 하나를 타겟파일(dest) 하나로 미니파이(minify)하는 어그리파이(uglify) 타겟명인 `build`를 지정했다.

```js
// 프로젝트 환경설정.
grunt.initConfig({
  pkg: grunt.file.readJSON('package.json'),
  uglify: {
    options: {
      banner: '/*! <%= pkg.name %> <%= grunt.template.today("yyyy-mm-dd") %> */\n'
    },
    build: {
      src: 'src/<%= pkg.name %>.js',
      dest: 'build/<%= pkg.name %>.min.js'
    }
  }
});
```

### Loading Grunt plugins and tasks
자주 사용하는 [concatenation], [minification], [linting]같은 task들은 [grunt plugins]으로 설정해서 사용할 수 있다. 즉, `package.json`의 dependency에 플러그인을 설정했다면, `npm install`을 통해서 이미 설치된 것이므로, 간단한 코드 추가로 `Gurntfile`에서 사용 가능하다.

```js
// "uglify" task를 지원하는 플러그인 로드.
grunt.loadNpmTasks('grunt-contrib-uglify');
```

**참고**: `grunt --help` 명령어를 사용하면 사용 가능한 taks 목록을 볼 수 있다.

### Custom tasks
`default` task를 설정하면 기본으로 실행할 task를 하나 혹은 그 이상 지정할 수 있다. 예를 들면, 컴맨드 라인에 특정 task를 지정하지 않고 `grunt`만 입력하면 `uglify` task가 실행된다. 이는 실제로는 `grunt uglify`나 `grunt default`와 같다. 배열 안에는 다수의 task를 지정할 수도 있다. (아규먼트는 있을 수도 있고 없을 수도 있다.)

```js
// Default task(s).
grunt.registerTask('default', ['uglify']);
```

만약 프로젝트에서 Grunt 플러그인 목록에 없는 task가 필요하다면, `Gruntfile`에 직접 custom task를 만들 수도 있다. 예를 들면, 다음 `Gruntfile`은 task configuration을 전혀 활용하지 않는 custom `default` task를 정의한다.

```js
module.exports = function(grunt) {

  // default task의 기본형.
  grunt.registerTask('default', 'Log some stuff.', function() {
    grunt.log.write('Logging some stuff...').ok();
  });

};
```

프로젝트 전용 task라면 `Gruntfile`안에 정의할 필요없이. 별도의 `.js` 파일로 정의하고 [[grunt.loadTasks|grunt#grunt.loadtasks]] 메서드로 불러와도 된다.

## Further Reading
* [[Installing grunt]] 가이드는 인스톨 스펙, 프로덕션, 개발, Grunt와 grunt-cli의 버전에 대한 상세한 정보를 제공한다.
* [[Configuring Tasks]] 가이드는 `Gruntfile`에서 task, target, option, file을 설정하는 방법을 상세하게 설명한다. tempaltes, globbing pattern, importing external data도 설명한다.
* [[Creating Tasks]] 가이드에는 Grunt task의 타입간의 차이점 목록이 있으며, task와 configuration의 샘플도 제공한다.
* custom task나 Grunt 플러그인에 대한 더 많은 정보는 [[developer documentation|grunt]]을 확인하자.


[npm]: https://npmjs.org/
[devDependencies]: https://npmjs.org/doc/json.html#devDependencies
[json]: https://npmjs.org/doc/json.html
[npm init]: https://npmjs.org/doc/init.html
[grunt-init]: Project-Scaffolding
[tilde version range]: https://npmjs.org/doc/json.html#Tilde-Version-Ranges
[grunt-contrib-uglify]: http://github.com/gruntjs/grunt-contrib-uglify
[concatenation]: https://github.com/gruntjs/grunt-contrib-concat
[linting]: https://github.com/gruntjs/grunt-contrib-jshint
[grunt.loadTasks]: https://github.com/gruntjs/grunt/wiki/grunt.task
