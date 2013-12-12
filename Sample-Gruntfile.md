다음 다섯 개의 Grunt 플러그인을 사용하는 `Gruntfile` 샘플을 살펴보겠다.

- [grunt-contrib-uglify](https://github.com/gruntjs/grunt-contrib-uglify)
- [grunt-contrib-qunit](https://github.com/gruntjs/grunt-contrib-qunit)
- [grunt-contrib-concat](https://github.com/gruntjs/grunt-contrib-concat)
- [grunt-contrib-jshint](https://github.com/gruntjs/grunt-contrib-jshint)
- [grunt-contrib-watch](https://github.com/gruntjs/grunt-contrib-watch)

전체 `Gruntfile`은 이 페이지 가장 아래에 제공하지만, 순서대로 읽어야 단계별로 이해할 수 있다.

첫 부분은 Grunt 환경설정을 캡슐화하는 "wrapper" 함수다.

```javascript
module.exports = function(grunt){
}
```

이 함수안에서 환경설정 객체를 생성할 수 있다.

```javascript
grunt.initConfig({
});
```

다음은 `pkg`프로퍼티에 `package.json`파일의 프로젝트 설정을 읽어서 설정한다. 이를 통해서 `package.json` 파일의 프로퍼티 값들을 참조할 수 있다. 조금만 더 읽으면 볼 수 있다.

```javascript
pkg: grunt.file.readJSON("package.json");
```

여기까지 본 것을 합치면 이렇게 된다.

```javascript
module.exports = function(grunt) {
  grunt.initConfig({
    pkg: grunt.file.readJSON('package.json')
  });
};
```

이제, task 별 환경설정을 정의할 수 있다. task를 위한 환경설정 객체는 전체 환경설정 객체에서 task 명과 동일한 이름의 프로퍼티로 존재한다. 즉, "concat" task는 환경설정 객체의 "concat" 프로퍼티에 있다. 다음은 "concat"을 위한 task 환경설정 객체의 예제다.

```javascript
concat: {
  options: {
    // 합친 결과 파일에서 각 파일을 구분할 문자열을 정의한다.
    separator: ';'
  },
  dist: {
    // 합칠 파일들.
    src: ['src/**/*.js'],
    // 결과 js 파일의 위치
    dest: 'dist/<%= pkg.name %>.js'
  }
}
```

`package.json`의 `name` 프로퍼티를 어떻게 참조했는지 보이는가? 우리는 `package.json`을 불러온 결과가 들어있는 `pkg` 프로퍼티를 통해서 `pkg.name`로 접근했다. 이 값은 이미 자바스크립트 객체로 파싱되어있다. Grunt는 환경설정 객체의 프로퍼티 값을 뱉어내는 탬플릿 엔진을 가지고 있다. 여기서는 `src/` 폴더에서 확장자가 `.js`인 파일 전부를 합치는 concat task를 설정했다.

이번에는 자바스크립트를 minify(공백을 제거하고 변수명을 짧은 이름으로 바꾸는 작업)하는 uglify 플러그인을 설정해보자.

```javascript
uglify: {
  options: {
    // 결과 파일 상단에 주석을 넣는다.
    banner: '/*! <%= pkg.name %> <%= grunt.template.today("dd-mm-yyyy") %> */\n'
  },
  dist: {
    files: {
      'dist/<%= pkg.name %>.min.js': ['<%= concat.dist.dest %>']
    }
  }
}
```

이제 uglify task는 minify한 자바스크립트 파일을 `dist/`에 생성한다. 여기서는 concat task에서 생성한 파일을 사용해서 minify를 진행하기 위해서 `<%= concat.dis.dest %>`를 사용했다.

QUnit 플러그인은 설정이 정말 간단한다. 그냥 QUnit을 실행하는 HTML 실행 파일(test runner)이 있는 위치만 지정하면 된다.

```javascript
qunit: {
  files: ['test/**/*.html']
},
```

JSHint 플러그인도 정말 간단한다.

```javascript
jshint: {
  // 검사를 실행할 파일을 지정한다.
  files: ['gruntfile.js', 'src/**/*.js', 'test/**/*.js'],
  // JSHint 환경설정 (http://www.jshint.com/docs/ 참고)
  options: {
    // 바꾸고 싶은 JSHint 기본값을 여기 지정한다.
    globals: {
      jQuery: true,
      console: true,
      module: true
    }
  }
}
```

JSHint도 파일 목록이 담긴 배열과 옵션 객체만 설정하면 된다. JSHint에 대한 정보는 [사이트](http://www.jshint.com/docs/)를 참고한다. JSHint를 기본값으로 잘 써왔다면, Gruntfile에서 재정의 할 필요없다.

끝으로 watch 플러그인을 보자.

```javascript
watch: {
  files: ['<%= jshint.files %>'],
  tasks: ['jshint', 'qunit']
}
```

이 task는 컴멘드 라인에 `grunt watch`를 입력해서 실행할 수 있다. 그러면, 설정한 파일에 어떤 변화가 감지될 때, 지정한 task들을 순서대로 실행한다. (여기서는 JSHint 확인을 위해서 같은 대상파일을 사용했다.)

이제, 필요한 Grunt 플러그인을 불러와야 하는데, 이들은 사전에 npm을 통해서 설치되어있어야 한다.

```javascript
grunt.loadNpmTasks('grunt-contrib-uglify');
grunt.loadNpmTasks('grunt-contrib-jshint');
grunt.loadNpmTasks('grunt-contrib-qunit');
grunt.loadNpmTasks('grunt-contrib-watch');
grunt.loadNpmTasks('grunt-contrib-concat');
```

그리고 나서 task를 몇 개 설정한다. 가장 중요한건 default task다.

```javascript
// 컴멘드 라인에 "grunt test"를 입력하면 실행된다.
grunt.registerTask('test', ['jshint', 'qunit']);

// default task는 컴멘드 라인에 "grunt"만 입력했을 때 실행할 task들이다.
grunt.registerTask('default', ['jshint', 'qunit', 'concat', 'uglify']);
```

전체 `Gruntfile.js`다.

```javascript
module.exports = function(grunt) {

  grunt.initConfig({
    pkg: grunt.file.readJSON('package.json'),
    concat: {
      options: {
        separator: ';'
      },
      dist: {
        src: ['src/**/*.js'],
        dest: 'dist/<%= pkg.name %>.js'
      }
    },
    uglify: {
      options: {
        banner: '/*! <%= pkg.name %> <%= grunt.template.today("dd-mm-yyyy") %> */\n'
      },
      dist: {
        files: {
          'dist/<%= pkg.name %>.min.js': ['<%= concat.dist.dest %>']
        }
      }
    },
    qunit: {
      files: ['test/**/*.html']
    },
    jshint: {
      files: ['Gruntfile.js', 'src/**/*.js', 'test/**/*.js'],
      options: {
        // options here to override JSHint defaults
        globals: {
          jQuery: true,
          console: true,
          module: true,
          document: true
        }
      }
    },
    watch: {
      files: ['<%= jshint.files %>'],
      tasks: ['jshint', 'qunit']
    }
  });

  grunt.loadNpmTasks('grunt-contrib-uglify');
  grunt.loadNpmTasks('grunt-contrib-jshint');
  grunt.loadNpmTasks('grunt-contrib-qunit');
  grunt.loadNpmTasks('grunt-contrib-watch');
  grunt.loadNpmTasks('grunt-contrib-concat');

  grunt.registerTask('test', ['jshint', 'qunit']);

  grunt.registerTask('default', ['jshint', 'qunit', 'concat', 'uglify']);

};
```








