## grunt-init
Grunt-init는 자동으로 프로젝트를 생성하는 사용하는 스캐폴딩(scaffolding) 도구다. 몇 가지 질문에 답하면, 지금 환경에 디렉터리 구조 전체를 생성한다. 질문에 답을 해서 템플릿을 선택하면 이에 따라 파일과 그 내용이 생성된다. 
질문에 어떻게 답하느냐에 따라 템플릿이 선택되는데, 생성되는 파일과 내용은 이를 기반으로 한다. 

_주의: 이 유틸리티는 Grunt에 내장된 "init" task를 사용한다. 자세한 정보는 [Upgrading from 0.3 to 0.4](https://github.com/gruntjs/grunt/wiki/Upgrading-from-0.3-to-0.4)를 참고하자._

## Installation
먼저 전역 유틸리티로 grunt-init를 설치한다. 

```shell
npm install -g grunt-init
```

이제 `grunt-init`란 컨맨드 명령어가 여러분의 시스템 경로에 설정되었고, 어느 폴더에서나 실행할 수 있다. 

_주의: 이를 위해서 sudo가 필요하거나 쉘을 Administrator 권한으로 실행해야 할 수도 있다._

## Usage
* `grunt-init --help`로 도움말과 사용할 수 있는 템플릿 목록을 볼 수 있다. 
* `grunt-init TEMPLATE`로 지정한 템플릿 기반의 신규 프로젝트 생성을 할 수 있다. 
* `grunt-init /path/to/TEMPLATE`라고 하면 별도 위치의 템플릿 기반의 신규 프로젝트 생성을 할 수 있다. 

대부분의 템플릿은 명령어를 날리면 현재 폴더에 관련 파일을 생성한다. 그러니 실수하고 싶지 않으면 먼저 새 폴더로 이동하자.

## Installing templates
템플릿이 `~/.grunt-init/` 폴더(윈도는 `%USERPROFILE%\.grunt-init\`)에 설치되면 grunt-init로 바로 사용할 수 있다. 템플릿을 이 폴더에 넣을 때는 git clone을 사용하는 게 좋다. 예를 들어 [grunt-init-jquery](https://github.com/gruntjs/grunt-init-jquery) 템플릿은 다음처럼 설치할 수 있다. 

```
git clone https://github.com/gruntjs/grunt-init-jquery.git ~/.grunt-init/jquery
```

_주의: "foobarbaz"라는 나만의 템플릿을 만들려면 git으로 클론 받은 템플릿과 함께 `~/.grunt-init/foobarbaz`를 지정하면 된다. grunt-init는 템플릿을 실행할 때 `~/.grunt-init/` 폴더 안에 있는 템플릿 폴더명을 사용한다._

grunt-init는 공식적으로 다음 템플릿들을 제공한다. 

* [grunt-init-commonjs](https://github.com/gruntjs/grunt-init-commonjs) - commonjs 모듈을 생성한다. 유닛 테스트는 nodeunit를 사용한다.   ([이 템플릿으로 "생성된" 모듈 샘플 repo](https://github.com/gruntjs/grunt-init-commonjs-sample/tree/generated) | [creation transcript](https://github.com/gruntjs/grunt-init-commonjs-sample#project-creation-transcript))
* [grunt-init-gruntfile](https://github.com/gruntjs/grunt-init-gruntfile) - 기본 Gruntfile를 생성한다.. ([이 템플릿으로 "생성된" 샘플 repo](https://github.com/gruntjs/grunt-init-gruntfile-sample/tree/generated) | [creation transcript](https://github.com/gruntjs/grunt-init-gruntfile-sample#project-creation-transcript))
* [grunt-init-gruntplugin](https://github.com/gruntjs/grunt-init-gruntplugin) - grunt 플러그인을 생성한다. 유닛 테스트는 Nodeunit를 사용한다.  ([이 템플릿으로 "생성된" 샘플 repo](https://github.com/gruntjs/grunt-init-gruntplugin-sample/tree/generated) | [creation transcript](https://github.com/gruntjs/grunt-init-gruntplugin-sample#project-creation-transcript))
* [grunt-init-jquery](https://github.com/gruntjs/grunt-init-jquery) - jQuery 플러그인을 생성한다. 유닛 테스트는 QUnit을 사용한다. ([이 템플릿으로 "생성된" 샘플 repo](https://github.com/gruntjs/grunt-init-jquery-sample/tree/generated) | [creation transcript](https://github.com/gruntjs/grunt-init-jquery-sample#project-creation-transcript))
* [grunt-init-node](https://github.com/gruntjs/grunt-init-node) - Node.js 모듈을 생성한다. 유닛 테스트는 Nodeunit를 사용한다.  (["생성된" 모듈 샘플 repo](https://github.com/gruntjs/grunt-init-node-sample/tree/generated) | [creation transcript](https://github.com/gruntjs/grunt-init-node-sample#project-creation-transcript))

## Custom templates
나만의 템플릿을 생성하고 사용할 수 있다. 이런 사용자 정의 템플릿은 앞에서 언급한 템플릿과 구조가 같아야 한다.

`my-template`라는 샘플 템플릿이 있다고 하면, 이 템플릿은 다음 파일 구조를 따른다. 

* `my-template/template.js` - 이 템플릿의 메인 파일
* `my-template/rename.json` - 템플릿 동작 시 템플릿 명세의 파일 이름 변경 규칙.
* `my-template/root/` - 목적지로 복사할 파일들

이 파일들이 `/path/to/my-template`에 있다고 가정하면, 이 템플릿은 `grunt-init /path/to/my-template` 명령어로 사용할 수 있다. 각자 고유한 이름을 가진 템플릿이 같은 폴더에 여럿 있을 수도 있다. 

커스텀 템플릿이 `~/.grunt-init/` 폴더(윈도는 `%USERPROFILE%\.grunt-init\`)에 있다면 `grunt-init my-template`라고만 적어도 자동으로 동작한다. 

### Copying files
템플릿 하위 폴더 `root/`에 있는 파일이 init용 템플릿이 실행될 때 현재 폴더로 복사되게 하려면, 템플릿에서 `init.filesToCopy`와 `init.copyAndProcess` 메서드를 사용하면 된다. 

`noProcess` 옵션이 없는 한, 복사된 모든 파일은 `props` 데이터 객체 기반으로 처리해야할 `{% %}` 템플릿을 처리해서 템플릿화 된다. [jquery template](https://github.com/gruntjs/grunt-init-jquery)을 참고하자.

### Renaming or excluding template files
템플릿 파일명을 바꾸거나 무시하려면, `rename.json`을 사용한다. `rename.json`에는 `sourcepath`와 `destpath`간의 명명 매핑을 기술할 수있다. `sourcepath`에는 `root/`를 기준으로한 복사될 파일 목록에 있는 파일명이 와야한다. 반면에 `destpath`에서는 목적 경로를 기술하기 위해서 `{% %}` 템플릿을 사용할 수 있다. 

파일을 복사하고 싶지 않다면 `destpath`에 `false`을 지정한다. `sourcepath`에서는 glob 패턴을 쓸 수 있다. 

## Specifying default prompt answers
커맨드 라인에서 grunt-init를 실행할 때, 몇가지 질문에 답을 해야 한다. 각 질문은 하드 코딩된 기본값이 있거나, 해당 환경에서 그 값을 찾는다. 만약 특정 기본값을 덥어쓰고 싶다면, `~/.grunt-init/defaults.json` 파일을 적용한다. (윈도는 `%USERPROFILE%\.grunt-init\defaults.json`파일)

예를 들어, 기본 설정된 이름과 다른 이름을 사용하고 싶고, 이메일 주소는 쓰고 싶지 않고, 자동으로 url이 지정되길 원한다면 다음처럼 `defaults.json` 파일을 만든다.

```json
{
  "author_name": "\"Cowboy\" Ben Alman",
  "author_email": "none",
  "author_url": "http://benalman.com/"
}
```

_주의: 내장된 프롬프트 질문 목록에 대한 문서는 아직 없다. 문서가 나올 때 까지는 속성 명과 기본값을 [소스코](https://github.com/gruntjs/grunt-init/blob/master/tasks/init.js)에서 확인하자._

## Defining an init template
init 템플릿을 정의해보자. 

### exports.description
사용자가 `grunt init`나 `grunt-init`를 실행했을 때, 사용 가능한 init 템플릿 목록이 출력되는데 거기서 템플릿 명과 같이 보여줄 부연설명이다. 

```js
exports.description = descriptionString;
```

### exports.notes
추가 설명이다. 필수 입력 프로퍼티는 아니지만, 질문 입력을 하는 프롬프트보다 먼저 출력되기 때문에, 사용자에게 입력 규칙과 필수, 비필수을 짧게 제공하기에 적합하다. 

```js
exports.notes = notesString;
```

### exports.warnOn
이 프로퍼티는 필수는 아니지만, 사용하는 게 좋다. Grunt은 템플릿 파일을 복사할 때, 이 프로퍼티에 지정한 패턴이나 패턴 배열과 일치하는 파일이 있으면,  경고를 뱉고 복사를 취소한다. 물론 사용자가 `--force`로 강제로 덥어쓸 수 있기는 하다. 이미 있는 파일을 덥어쓰지 않기를 원할 때 유용하다.

```js
exports.warnOn = wildcardPattern;
```

일반적으로 모든 파일과 폴더를 의미하는 `'*'`는 물론이고, [minimatch](https://github.com/isaacs/minimatch)를 통해서 좀 더 유연한 와일드 카드 패턴을 제공한다. 

```js
exports.warnOn = 'Gruntfile.js';        // Gruntfile.js.
exports.warnOn = '*.js';            // 모든 .js 파일.
exports.warnOn = '*';               // 점(dot)으로 시작하지 않는 모든 파일과 폴더
exports.warnOn = '.*';              // 점(dot)으로 시작하는 모든 파일과 폴더
exports.warnOn = '{.*,*}';          // 점(dot)에 관계없이 모든 파일과 폴더
exports.warnOn = '!*/**';           // 모든 파일(폴더는 아님)
exports.warnOn = '*.{png,gif,jpg}'; // 모든 이미지 파일

// 이미지 파일 예제의 다른 표현
exports.warnOn = ['*.png', '*.gif', '*.jpg'];
```

### exports.template
`exports`에는 프로퍼티가 여럿 있지만, 이 프로퍼티에 할당된 함수가 템플릿을 생성하는 실제 코드다. 
이 함수는 3개의 인자를 갖는다. 첫 인자 `grunt`는 grunt의 모든 [메서드와 라이브러리](api/grunt)를 가진 참조 객체고, `init` 인자는 현재 init 템플릿에서 지정한 프로퍼티와 메서드를 가진 객체며, `done` 인자는 init 템플릿 실행을 끝내고 호출할 콜백 함수다.

```js
exports.template = function(grunt, init, done) {
  // 다음 세션인 "Inside an init template"을 참고하자.
};
```

## Inside an init template

### init.addLicenseFiles
files 객체에 적절한 라이센스 파일을 추가한다.

```js
var files = {};
var licenses = ['MIT'];
init.addLicenseFiles(files, licenses);
// files === {'LICENSE-MIT': 'licenses/LICENSE-MIT'}
```

### init.availableLicenses
사용 가능한 라이센스들을 배열로 반환한다. 

```js
var licenses = init.availableLicenses();
// licenses === [ 'Apache-2.0', 'GPL-2.0', 'MIT', 'MPL-2.0' ]
```

### init.copy
주어진 대상 파일(절대, 상대경로)을 목적지(상대 경로)로 복사한다. 목적지는 필수가 아닌 옵션값이다. `template` 콜백함수 `done`에서 필요에 따라 처리된다.

```js
init.copy(srcpath[, destpath], options)
```

### init.copyAndProcess
인자로 전달한 객체의 모든 파일(`files`)을 순회하면서 대상 파일을 `{% %}` 템플릿을 처리해서 목적지로 복사한다. 

```js
init.copyAndProcess(files, props[, options])
```

### init.defaults
`defaults.json`라는 별도 파일에 정의한 질의 프롬프트의 사용자 지정 기본값.

```js
init.defaults
```

### init.destpath
목적지 파일의 절대 경로

```js
init.destpath()
```

### init.expand
[grunt.file.expand](https://github.com/gruntjs/grunt/wiki/grunt.file#wiki-grunt-file-expand)과 동일하다.

주어진 와일드 카드 패턴과 일치하는 파일 배열, 폴더 경로를 중복을 제거해서 반환한다. 이 메서드는 콤마(,)로 구분했거나 배열로 된 와일드 카드 페턴을 인자로 사용한다. 느낌표(!)로 시작하는 패턴과 일치하는 경로는 반환 값에서 제외된다. 패턴은 순서대로 처리되므로, 느낌표(제거를 위해) 사용에 주의해야 한다. 

```js
init.expand([options, ] patterns)
```

### init.filesToCopy
복사할 파일 목록을 담고 있는 객체를 반환한다. 이 객체는 rename.json 규칙에 따라서 이름 변경을 한 대상 파일(절대 경로)과 목적지(상대 경로)를 담고 있다. rename.json이 없으면 이름 변경은 하지 않는다. 

```js
var files = init.filesToCopy(props);
/* files === { '.gitignore': 'template/root/.gitignore',
  '.jshintrc': 'template/root/.jshintrc',
  'Gruntfile.js': 'template/root/Gruntfile.js',
  'README.md': 'template/root/README.md',
  'test/test_test.js': 'template/root/test/name_test.js' } */
```

### init.getFile
단일 task 파일 경로를 가져온다.

```js
init.getFile(filepath[, ...])
```

### init.getTemplates
사용 가능한 모든 템플릿을 담은 객체를 반환한다. 

```js
init.getTemplates()
```

### init.initSearchDirs
init 템플릿을 찾을 폴더를 생성한다. `template`은 템플릿의 위치다. 여기에는 `~/.grunt-init/`와 grunt-init 안에 있는 주요 init task들이 포함된다.

```js
init.initSearchDirs([filename])
```

### init.process
입력 프롬프트를 대기할 프로세스를 시작한다. 

```js
init.process(options, prompts, done)
```

```js
init.process({}, [
  // 아래 값들을 위한 프롬프트
  init.prompt('name'),
  init.prompt('description'),
  init.prompt('version')
], function(err, props) {
  // 모든 입력이 끝나고, 그 프로퍼티와 관련된 일을 여기서 한다.
});
```

### init.prompt
입력값을 위한 사용자 프롬프트

```js
init.prompt(name[, default])
```

### init.prompts
프롬프트 모두를 담은 객체

```js
var prompts = init.prompts;
```

### init.readDefaults
task 파일이 있는 경우 거기서 JSON 기본값을 읽어와서 한 데이터 객체로 합친다. 

```js
init.readDefaults(filepath[, ...])
```

### init.renames
해당 템플릿의 이름 변경 규칙

```js
var renames = init.renames;
// renames === { 'test/name_test.js': 'test/{%= name %}_test.js' }
```

### init.searchDirs
템플릿을 찾을 폴더 배열

```js
var dirs = init.searchDirs;
/* dirs === [ '/Users/shama/.grunt-init',
  '/usr/local/lib/node_modules/grunt-init/templates' ] */
```

### init.srcpath
init 템플릿 경로에서 filename을 찾아서 절대 경로를 반환한다. 

```js
init.srcpath(filepath[, ...])
```

### init.userDir
사용자 템플릿 폴더의 절대 경로를 반환한다. 

```js
var dir = init.userDir();
// dir === '/Users/shama/.grunt-init'
```

### init.writePackageJSON
목적지 폴더에 package.json 파일을 저장한다. callback 함수는 사후처리로 프로퍼티를 추가, 삭제등의 작업할 때 사용한다. 

```js
init.writePackageJSON(filename, props[, callback])
```

## Built-in prompts

### author_email
`package.json`에 사용할 작성자 이메일 주소. 여기에 사용할 기본값은 사용자의 git config에서 먼저 찾는다.

### author_name
`package.json`에서 사용할 작성자 이름. 여기에 사용할 기본값은 사용자의 git config에서 먼저 찾는다.

### author_url
`package.json`에서 사용할 작성자 웹사이트 URL.

### bin
cli 스크립트를 위한 프로젝트 상대 경로.

### bugs
프로젝트 이슈 트랙커의 URL. 프로젝트 레파지토리가 github이면 github 이슈 트랙커를 기본값으로 사용한다. 

### description
`package.json`과  README 파일에서 사용할 프로젝트 부연설명. 

### grunt_version
프로젝트에 필요한 Grunt 버전으로, 이 버전은 유효한 semantic version로 범위를 기술해야 한다. 

### homepage
프로젝트 홈페이지 URL. 프로젝트 레파지토리가 github이면 github URL을 기본값으로 사용한다. 

### jquery_version
jQuery 프로젝트인 경우, 프로젝트에 필요한 jQuery 버전. 이 버전은 유효한 semantic version로 범위를 기술해야 한다. 

### licenses
프로젝트의 라이센스(들), 라이센스가 여러 개인 경우 공백으로 구분한다. `MIT`, `MPL-2.0`, `GPL-2.0`, `Apache-2.0` 라이센스가 내장되어있고, `MIT`가 기본값이다. 추가 라이센스는 [init.addLicenseFiles](#initaddlicensefiles)로 추가한다. 

### main
이 프로젝트의 진입점. 기본값은 `lib` 폴더 안의 프로젝트 명이다. 

### name
프로젝트 명. 이 이름은 프로젝트 템플릿 전체에서 사용된다. 기본값은 해당 작업 폴더 명이다. 

### node_version
프로젝트에 필요한 Node.js의 버전. 이 버전은 유효한 semantic version로 범위를 기술해야 한다. 

### npm_test
프로텍트의 테스트 코드를 실행할 커맨드 명령어. 기본값은 `grunt`다. 

### repository
프로젝트의 git 레파지토리. 기본값은 github URL이라고 가정한다. 

### title
사람이 읽기 좋게 바꾼 프로젝트 명. `name` 값을 기본값으로 가독성 좋게 바꿔서 사용한다. 

### version
프로젝트 버전. 기본값은 유효한 semantic version인 `0.1.0`이다.
