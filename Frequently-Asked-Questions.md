## How do I install grunt?
일반적인 설치 과정은 [[Getting Started]]을 참고하고, 그러고 나서도 궁금한 게 있다면 [[Installing grunt]]를 보자.

## When will I be able to use in-development feature 'X'?
개발단계의 어떤 기능을 사용할 수 있는 시기가 궁금한가? Grunt는 공식 배포 버전과 개발 버전 모두 설치 가능하며, [[Installing grunt]] 에서 다룬다.


## Does Grunt work on Windows?
Window에서도 Grunt를 사용할 수 있다. [Node.js](http://nodejs.org/)와 [npm](http://npmjs.org/)가 Window에서 잘 동작하므로 Grunt도 문제없이 동작한다. Node.js 구버전이 [Cygwin](http://www.cygwin.com/)에 번들 되어있어서 보통 문제는 Cygwin에서 생긴다.

이 문제를 안 겪으려면 `git`은 [msysGit installer](http://msysgit.github.com/)으로, `node`와 `npm`은 [Node.js installer](http://nodejs.org/#download)으로 설치한다. 그리고 쉘은 Cygwin대신에 [Windows command prompt](http://www.cs.princeton.edu/courses/archive/spr05/cos126/cmd-prompt.html)나 [PowerShell](http://support.microsoft.com/kb/968929)을 사용한다.

## Why doesn't my asynchronous task complete?
비동기 task가 완료되지 않는 이유는 여러분이 [this.async](grunt.task#wiki-this-async) 메서드 호출을 안 해서 Grunt가 여러분의 task가 비동기 모드인걸 모르기 때문이다. 쉽게 하려면, 동기식 코딩 스타일을 사용하고 나서 task 내부에 'this.async()'를 호출해서 비동기로 변경할 수 있다.

Note that passing `false` to the `done()` function tells Grunt that the task has failed.
`done()` 함수에 인자 `false`을 넘겨서 task를 실패 처리할 수 있다.

```javascript
grunt.registerTask('asyncme', 'My asynchronous task.', function() {
  var done = this.async();
  doSomethingAsync(done);
}); $scope
```

## How do I enable shell tab auto-completion?
Grunt도 쉘에서 자동완성 기능을 쓸 수 있다. 다음 코드를 `~/.bashrc`에 추가하자.

```bash
eval "$(grunt --completion=bash)"
```

단, `npm install -g grunt`로 Grunt가 이미 설치되어있어야 하고, 아직 bash밖에 지원하지 않는다.

## How can I share parameters across multiple tasks?
multiple task 끼리 파라미터를 공유하려면 어떻게 해야 할까? task는 모두 자기 파라미터만 접근할 수 있다. multiple task 간에 파라미터를 공유하기 위해서는 몇 가지 옵션이 필요하다.

### "Dynamic" alias tasks
**multiple task 간 파라미터를 공유하려 할 때 가장 자주 쓰이는 방법은 동적(dynamic)인 alias task이다.**

[alias task](grunt#grunt.registertask)는 원래 형태가 간단하다. 일반 task도 [grunt.task.run](grunt.task#grunt.task.run)를 사용하면 좀 더 효율적인 형태의 "동적인" alias task로 만들 수 있다. 예를 들어, 다음 예제는 `grunt build:001`이라고 컴멘드를 입력하면 `foo:001`, `bar:001`, `baz:001` task가 실행된다.

```javascript
grunt.registerTask('build', '빌드 task를 모두 실행해라.', function(n) {
  if (n == null) {
    grunt.warn('build:001 처럼 빌드 넘버를 반드시 지정해야 한다.');
  }
  grunt.task.run('foo:' + n, 'bar:' + n, 'baz:' + n);
});
```

### -- options

[grunt.option](grunt#wiki-grunt-option)를 사용해도 multiple task 간에 파라미터를 공유할 수 있다. 예를 들어, `grunt deploy --target=staging`을 컴멘트에 입력하면 `grunt.option('target')`으로 target 값인 `"staging"`에 접근할 수 있다.

```javascript
grunt.registerTask('upload', '지정한 위치로 코드를 업로드한다.', function(n) {
  var target = grunt.option('target');
  // target으로 어떤 작업을 한다.
});
grunt.registerTask('deploy', ['validate', 'upload']);
```

_블리언(boolean) 옵션의 경우는 값을 적지 않고 키만 지정할 수도 있다. `grunt deploy --staging`을 입력하면 `grunt.option('staging')은 `true`를 반환한다._

### Globals and configs

때로는 환경설정이나 전역 변수값을 지정하고 싶을 수도 있다. 이런 경우는 전역 변수나 환경설정 변수로 task 파라미터를 지정하는 task를 만들어라.

예를 들어, `grunt set_global:name:peter set_config:target:staging deploy`라는 컴멘트 명령어를 입력하면 `global.name`은 `"peter"`가 되고, `grunt.config('target')`은 `"staging"`을 반환한다. 이제 `deploy` task에서 이 값을 쓸 수 있다.

```javascript
grunt.registerTask('set_global', '전역변수을 지정한다.', function(name, val) {
  global[name] = val;
});

grunt.registerTask('set_config', '환경설정 프로퍼티를 지정한다.', function(name, val) {
  grunt.config.set(name, val);
});
```


***


## grunt 0.3 Questions

## On Windows with Grunt 0.3, why does my JS editor open when I try to run grunt?
윈도우에서는 grunt를 실행하면 js 개발용 에디터가 열린다. [Gruntfile](Getting-started)이 있는 폴더에서 grunt를 실행하면, 윈도가 그 파일을 실행하려고 시도하기 때문이다.이럴 때는 `grutn.cmd`를 사용하자.

아니면 [이 글](http://devblog.point2.com/2010/05/14/setup-persistent-aliases-macros-in-windows-command-prompt-cmd-exe-using-doskey/)을 참고해서 `DOSKEY` 컴멘트로 Grunt용 메크로를 만드는 방법도 있다. 그러면 `grunt.cmd`을 안쓰고 `grunt`를 사용할 수 있다.

`DOSKEY`는 이런식으로 사용한다.

```
DOSKEY grunt=grunt.cmd $*
```