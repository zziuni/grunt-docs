이 문서는 `Gruntfile`를 사용하는 프로젝트를 위해서 task 단위로 환경설정을 하는 법을 설명한다. `Gruntfile`에 대해 잘 모른다면 [[Getting Started]]문서를 먼저 읽어보고 [[Sample Gruntfile]] 파일을 체크아웃 받자.

## Grunt Configuration

task 환경설정은 `Gruntfile`에서 `grunt.initConfig` 메서드를 통해서 지정한다. 일반적으로 환경설정은 task 명과 동일한 프로퍼티의 값으로 설정한다. 여러분이 설정한 task의 이름들과 충돌하지만 않으면 얼마든지 임의의 데이터를 넣을 수 있다. 물론 무시되지만.

또한, 이 파일은 JSON이 아닌 자바스크립트 파일이기 때문에, 모든 자바스크립트 코드를 사용할 수 있다. 필요하다면 동적으로 환경설정을 생성할 수도 있다.

```js
    grunt.initConfig({
      concat: {
        // concat task를 위한 환경설정은 여기에 넣는다.
      },
      uglify: {
        // uglify task를 위한 환경설정은 여기에 넣는다.
      },
      // 특정 task와 관련 없는 임의의 프로퍼티
      my_property: 'whatever',
      my_src_files: ['foo/*.js', 'bar/*.js'],
    });
```


## Task Configuration and Targets

task 하나가 실행되면, Grunt는 그 task의 이름으로 `Gruntfile`의 환경설정 객체에서 프로퍼티를 찾고 이를 해당 task의 환경설정으로 사용한다. 여러 일을 한 번에 하는 multi-task는 다시 별도의 target 명을 사용해서 환경설정을 개별적으로 가질 수 있다. 다음 예제를 보면 `uglify` task는 `bar` target만 가지고 있지만, `concat` task는 `foo`와 `bar`라는 두 개의 target을 갖고 있다.

```js
    grunt.initConfig({
      concat: {
        foo: {
          // concat task의 "foo" 타겟을 위한 옵션과 파일을 여기에 넣는다.
        },
        bar: {
          // concat task의 "bar" 타겟을 위한 옵션과 파일을 여기에 넣는다.
        },
      },
      uglify: {
        bar: {
          // uglify task의 "bar" 타겟을 위한 옵션과 파일을 여기에 넣는다.
        },
      },
    });
```

`grunt concat`를 입력하면 모든 target을 순회하며, 차례대로 모든 target의 환경설정을 가져오는 반면에, `grunt concat:foo`나 `grunt concat:bar`처럼 task와 target을 모두 사용해서 지정하면 특정 target의 환경설정만 가져온다. 단, task 명이 [grunt.renameTask](https://github.com/gruntjs/grunt/wiki/grunt#wiki-grunt-renameTask)로 변경되면, Grunt는 새로운 task 명으로 config 객체에서 프로퍼티 명을 찾는다.


## Options

task 환경설정 하나를 살펴보자, `options` 프로퍼티는 내장된 기본값을 재정의 하기 위해서 덮어 쓸 때 사용한다. 그리고 각 target 별로도 해당 target에만 한정된 `options` 프로퍼티를 가질 수 있다. target 레벨의 options는 해당 타겟 레벨에서만 덮어써진다.

`options` 프로퍼티는 필수요소가 아니며, 필요없는 경우 무시된다.

```js
    grunt.initConfig({
      concat: {
        options: {
          // task 수준의 옵션 객체. task 기본값을 덥어쓴다.
        },
        foo: {
          options: {
            // "foo" target 수준의 옵션 객체. task 수준의 options을 덥어쓴다.
          },
        },
        bar: {
          // 명시된 options가 없다. 이 target은 task 수준의 options을 사용한다.
        },
      },
    });
```

## Files

대부분의 task는 파일을 대상으로하는 작업들이므로, Grunt는 task가 작업 대상 파일을 선언하기 위한 추상화된 기능을 강력하게 지원한다. **src-dest**(출처와 목적지)간의 파일 매핑을 정의하는 방법은 여러 가지가 있다. 매핑의 표현과 제어도 지속적으로 변경 가능하면서 말이다.
이제부터 설명할 유형들은 모든 task에서 사용할 수 있으므로, 여러분 상황에 맞는 포맷을 선택하면 된다.

`src`와 `dest`는 모두 파일 포맷에서 지원하지만 "Compact"와 "Files Array" 포맷에서는 몇 가지 추가 프로퍼티를 사용할 수 있다.

* `fileter`: 필터용도로 유효한 [fs.Stats 메서드명](http://nodejs.org/docs/latest/api/fs.html#fs_class_fs_stats)이나 함수를 적용한다. 이때, 함수는 적절한 `src` 파일경로를 받으면 `true`나 `false`를 반환해야 한다.
* `nonull`: `true`를 지정했을 때는 패턴과 일치하는 내용이 없는 경우, 그 패턴 자체를 담은 목록을 반환하고, `false`를 지정했을 때는 패턴과 일치하는 내용이 없는 경우, 빈 목록을 반환한다. `--verbos`와 함께 사용하면 파일 경로 관련 버그를 찾을 때 유용하다.
* `dot`: `true`면 패턴에 명시적으로 포함시키지 않아도 구두점(.)으로 시작하는 파일명을 일치시킨다.(src/*.js 하면 hidden files도 포함된다는 말.)
* `matchBase`: `true`면, 슬래시(/)를 포함하지 않은 패턴들이 슬래시를 가지고 있을 경로 기본명을 제외하고 매치된다. 예를 들어 `a?b` 패턴이 `/xyz/acb/123`이 아니라 `/xyz/123/acb`와 일치한다.
* `expand`은 동적인 src-dest 파일 매핑을 수행한다. 더 자세한 정보는 [Building the files object dynamically](configuring-tasks#building-the-files-object-dynamically)를 참고한다.
* 이 이외의 프로퍼티들은 매칭 옵션으로 하위 libs로 전달된다. 추가 옵션은 [node-glob][]와 [minimatch][]를 보자.



### Compact Format

이 형태는 target 별로 단일 **src-dest** 파일 매핑을 할 때 사용한다. 주로 [grunt-contrib-jshint](https://github.com/gruntjs/grunt-contrib-jshint)처럼 `dest`와 관계없이 `src` 프로퍼티 하나만 필요한 읽기 전용 task를 위해서 사용한다. src-dest 파일 매핑 별로 프로퍼티 확장도 가능하다.

```js
    grunt.initConfig({
      jshint: {
        foo: {
          src: ['src/aa.js', 'src/aaa.js']
        },
      },
      concat: {
        bar: {
          src: ['src/bb.js', 'src/bbb.js'],
          dest: 'dest/b.js',
        },
      },
    });
```

### Files Object Format

이 형태는 target 별로 다중 src-dest 매핑을 할 때 사용한다. 프로퍼티 명이 목적지(destination) 파일명이고 프로퍼티 값은 출처(source) 파일이 된다. 이 방법을 사용하면 src-dest 파일 매핑이 아무리 많아도 지정할 수 있다. 하지만 매핑 별로 추가 파라미터 지정은 불가능하다.

```js
    grunt.initConfig({
      concat: {
        foo: {
          files: {
            'dest/a.js': ['src/aa.js', 'src/aaa.js'],
            'dest/a1.js': ['src/aa1.js', 'src/aaa1.js'],
          },
        },
        bar: {
          files: {
            'dest/b.js': ['src/bb.js', 'src/bbb.js'],
            'dest/b1.js': ['src/bb1.js', 'src/bbb1.js'],
          },
        },
      },
    });
```

### Files Array Format

이 형태는 target 별로 다중 src-dest 매핑을 지원하면서 매핑 별로 추가 프로퍼티를 사용할 수 있다.

```js
    grunt.initConfig({
      concat: {
        foo: {
          files: [
            {src: ['src/aa.js', 'src/aaa.js'], dest: 'dest/a.js'},
            {src: ['src/aa1.js', 'src/aaa1.js'], dest: 'dest/a1.js'},
          ],
        },
        bar: {
          files: [
            {src: ['src/bb.js', 'src/bbb.js'], dest: 'dest/b/', nonull: true},
            {src: ['src/bb1.js', 'src/bbb1.js'], dest: 'dest/b1/', filter: 'isFile'},
          ],
        },
      },
    });
```

### Older Formats

**dest as targets** 파일 형태는 멀티 task와 target가 있기 전부터 있던 형태로 목적지(destination) 파일 경로가 그대로 target 명이 된다. 하지만 이 때문에 `grunt task:target`의 형태로 동작시키기가 거북할 수 있다. 또한 src-dest 매핑 별로 target 수준의 옵션이나 추가 프로퍼티를 지정할 수 없다.

이 형태는 되도록 사용하지 말고 가능하면 피하자.

```js
    grunt.initConfig({
      concat: {
        'dest/a.js': ['src/aa.js', 'src/aaa.js'],
        'dest/b.js': ['src/bb.js', 'src/bbb.js'],
      },
    });
```


### Custom Filter Function

`filter` 프로퍼티를 사용하면 파일을 좀 더 꼼꼼하게 지정할 수 있다. 그냥 적절한 [fs.Stats 메서드명](http://nodejs.org/docs/latest/api/fs.html#fs_class_fs_stats)을 사용하자. 다음 예제는 패턴과 일치하는 파일만 제거(clean)한다.

```js
    grunt.initConfig({
      clean: {
        foo: {
          src: ['tmp/**/*'],
          filter: 'isFile',
        },
      },
    });
```

아니면 파일 일치 여부에 따라 `true`, `false`를 반환하는 `filter`용 함수를 생성하자. 다음 예제는 빈 폴더만 지우는 필터다.

```js
    grunt.initConfig({
      clean: {
        foo: {
          src: ['tmp/**/*'],
          filter: function(filepath) {
            return (grunt.file.isDir(filepath) && require('fs').readdirSync(filepath).length === 0);
          },
        },
      },
    });
```

### Globbing patterns
대상(source) 파일 경로를 모두 개별적으로 지정하는 일이 불가능할 때도 있다. 그래서 Grunt는 [node-glob][]와 [minimatch][]라이브러리를 내부에 포함시켰고, 이를 통해서 파일명 확장을 지원한다. (globbing이라고도 한다.)

[node-glob]: https://github.com/isaacs/node-glob
[minimatch]: https://github.com/isaacs/minimatch

이 문서는 globbing 패턴 전반을 다루는 튜토리얼이 아니므로, 파일경로에서 사용할 수 있는 몇 가지만 소개한다.

* `*`는 개수와 관계없이 `/`를 제외한 모든 캐릭터와 일치한다.
* `?`는 `/`를 제외한 하나의 캐릭터와 일치한다.
* `**`는 개수와 관계없이 `/`를 포함한 모든 캐릭터와 일치한다. 하지만 경로부분(폴더명)에서만 동작한다.
* `{}`에 콤마로 구분된 목록을 넣으면 "or" 표현식으로 동작한다.
* `!`를 패턴의 처음에 사용하면 불일치(negative match)를 의미한다.

globbing 패턴을 잘 모르더라도 `foo/*.js`는 `foo/` 폴더에서 `.js`로 끝나는 모든 파일과 일치하지만, `foo/**/*.js`는 `foo/` 폴더와 그 아래의 모든 하위 폴더에서 `.js`로 끝나는 모든 파일과 일치한다는 것 정도는 알아두면 좋다.

globbing 패턴의 복잡함을 줄이기 위해서, Grunt에서는 파일 경로나 globbing 패턴을 배열로 지정할 수 있다. 패턴은 모두 배열의 색인 순서대로 처리되는데, 결과 집합에서 일치하는 파일을 제외하는 `!` 접두사 패턴도 마찬가지다. 최종 결과 집합은 중복값이 없는 유일값들이다.

예제을 보자.

```js
    // 단일 파일 지정.
    {src: 'foo/this.js', dest: ...}
    // 배열로 여러 파일 지정.
    {src: ['foo/this.js', 'foo/that.js', 'foo/the-other.js'], dest: ...}
    // glob 패턴 사용.
    {src: 'foo/th*.js', dest: ...}

    // 단일 node-glob 패턴.
    {src: 'foo/{a,b}*.js', dest: ...}
    // 물론 이렇게도 사용 가능.
    {src: ['foo/a*.js', 'foo/b*.js'], dest: ...}

    // foo/ 안의 모든 .js 파일. 순서는 알파벳 순.
    {src: ['foo/*.js'], dest: ...}
    // bar.js을 먼저 선택하고, 남은 파일을 알파벳 순으로 추가.
    {src: ['foo/bar.js', 'foo/*.js'], dest: ...}

    // bar.js를 제외한 모든 파일. 알파벳 순.
    {src: ['foo/*.js', '!foo/bar.js'], dest: ...}
    // 모든 파일을 알파벳 순서로 넣고, 끝에 bar.js를 추가.
    {src: ['foo/*.js', '!foo/bar.js', 'foo/bar.js'], dest: ...}

    // 파일경로나 glob 패턴에 템플릿(<%%>)을 사용할 수도 있다.
    {src: ['src/<%= basename %>.js'], dest: 'build/<%= basename %>.min.js'}
    // 환경설정의 다른 task의 target에서 정의한 파일 목록을 참조할 수도 있다.
    {src: ['foo/*.js', '<%= jshint.all.src %>'], dest: ...}
```

더 자세한 glob 패턴 문법은 [node-glob][]와 [minimatch][] 문서를 참고하자.


### Building the fiels object dynamically
대량의 파일들을 처리하고자 할 때, 파일 목록을 동적으로 생성하는데 사용가능한 몇 가지 추가 프로퍼티들이 있다. 이 프로퍼티들은 "Compact"와 "Files Array" 매핑 패턴에 지정할 수 있다.

* `expand`: 다음 옵션들을 활성화하려면 먼저 이 프로퍼티를 `true`로 설정한다.
* `cwd`: 모든 `src` 패턴을 이 옵션에 정의된 경로를 기준으로 정한다.
* `src`: 일치 여부 확인을 위한 패턴 목록. `cwd`기준 상대경로.
* `dest`: 목적지 지정을 위한 경로 접두사.
* `ext`: `dest` 경로에 생성할 파일의 확장자.
* `flatten`: `dest` 경로에 생성할 목록에서 경로부분을 제거하고 파일명만 남긴다.
* `rename`: 일치하는 `src` 파일별로 호출되는 함수. 이 함수는 (확장자 변경과 경로 제거 후) 일치하는 `src` 패턴과 `dest`를 인자로 받아서, 새로운 `dest` 값을 반환해야 한다. 동일 `dest` 값이 여러 번 반환되면, 사용된 `src`들이 이름변경을 위한 출처 배열에 추가된다.

다음 `minify` task 예제는 `static_mappings`와 `dynamic_mappings` target에서 동일한 src-dest 파일 매핑을 바라보고 있다. task를 실행하면, Grunt는 자동으로 `dynamic_mappings` 파일 목록을 정적인 4개의 src-dest 파일 매핑으로 바꾼다.

또한, 정적인 매핑과 동적인 매핑은 다양한 방법으로 함께 쓸 수 있다.

```js
    grunt.initConfig({
      minify: {
        static_mappings: {
          // 이 src-dest 파일 매핑은 수동으로 지정했기 때문에,
          // 파일 추가 삭제와 Grunfile 수정을 매번 해줘야 한다.
          files: [
            {src: 'lib/a.js', dest: 'build/a.min.js'},
            {src: 'lib/b.js', dest: 'build/b.min.js'},
            {src: 'lib/subdir/c.js', dest: 'build/subdir/c.min.js'},
            {src: 'lib/subdir/d.js', dest: 'build/subdir/d.min.js'},
          ],
        },
        dynamic_mappings: {
          // "minify" task가 실행되면 Grunt는 "lib/" 아래에서 "**/*.js"를 찾는다.
          // 그렇게 찾은 src-dest 파일 매핑으로 빌드한다.
          // 파일이 추가/제거 될 때 마다 Gruntfile을 수정할 필요없다.
          files: [
            {
              expand: true,     // 동적 기술법을 활성화.
              cwd: 'lib/',      // Src 패턴의 기준 폴더.
              src: ['**/*.js'], // 비교에 사용할 패턴 목록.
              dest: 'build/',   // 목적 경로의 접두사(사실상 폴더명)
              ext: '.min.js',   // dest의 파일들의 확장자.
            },
          ],
        },
      },
    });
```

## Templates
`<%%>` 구분자를 사용한 템플릿은 task가 해당 환경설정을 읽어올 때 자동으로 정적인 값으로 기술된다. 그리고 이는 템플릿를 모두 기술될 때 까지 재귀적으로 실행된다.

`grunt.initConfig()`의 환경설정 객체는 프로퍼티 설정시 컨텍스트로 사용되며, `grunt`와 그 메서드도 그 안에서 사용할 수 있다. 예) `<%= grunt.template.today('yyyy-mm-dd') %>`

* `<%= prop.subprop%>`는 환경설정 객체에서 `prop.subprop`을 찾아서 그 값으로 치환된다. 타입은 관계없다. 이같은 템플릿은 문자열 만이 아니라 배열과 다른 객체로 참조할 수도 있다.
* `<% %>`는 임의의 인라인 자바스크립트 코드를 실행한다. 그래서 흐름제어나 순회를 위해 사용할 수 있다.

여기 `concat` task가 있다. `grunt concat:sample`을 실행하면 `foo/*.js` + `bar/*.js` + `baz/*.js`와 일치하는 파일을 합치고 `/* abcde */` 상단 주석(banner)를 추가해서 `build/abcde.js`란 이름으로 생성한다.

```js
    grunt.initConfig({
      concat: {
        sample: {
          options: {
            banner: '/* <%= baz %> */\n',   // '/* abcde */\n'
          },
          src: ['<%= qux %>', 'baz/*.js'],  // [['foo/*.js', 'bar/*.js'], 'baz/*.js']
          dest: 'build/<%= baz %>.js',      // 'build/abcde.js'
        },
      },
      // task 환경설정 템플릿에 사용되는 임의의 프로퍼티들
      foo: 'c',
      bar: 'b<%= foo %>d', // 'bcd'
      baz: 'a<%= bar %>e', // 'abcde'
      qux: ['foo/*.js', 'bar/*.js'],
    });
```

## Importing External Data
다음 `Gruntfile` 파일은 `package.json` 파일에서 프로젝트 관련 메타데이터를 가져온다. 그리고 그 메타데이터를 [grunt-contrib-uglify 플러그인](http://github.com/gruntjs/grunt-contrib-uglify)의 `uglify` task의 상단 주석 생성과 src 파일 minify를 위한 정보로 사용한다.

Grunt는 JSON과 YAML 데이터를 가져올 수 있는 `grunt.file.readJSON`와 `grunt.file.readYAML`를 지원한다.

```js
    grunt.initConfig({
      pkg: grunt.file.readJSON('package.json'),
      uglify: {
        options: {
          banner: '/*! <%= pkg.name %> <%= grunt.template.today("yyyy-mm-dd") %> */\n'
        },
        dist: {
          src: 'src/<%= pkg.name %>.js',
          dest: 'dist/<%= pkg.name %>.min.js'
        }
      }
    });
```















