1. `npm install -g grunt-init`로 [grunt-init](https://github.com/gruntjs/grunt-init)를 설치한다.
2. `git clone git://github.com/gruntjs/grunt-init-gruntplugin.git ~/.grunt-init/gruntplugin`로 grutn plugin 템플릿을 설치한다.
3. 빈 폴더에서 `grunt-init gruntplugin`을 실행한다.
4. 개발 환경 구축을 위해 `npm install`을 실행한다.
5. 필요한 플러그인을 작성한다.
6. npm으로 만든 플러그인을 배포한다. `npm publish`

## Notes

### Naming your task

플러그인 제작시 여러분의 task명에 "grunt-contrib" 라는 네임스페이스는 사용하지 않았으면 좋겠다. 이 이름은 Grunt 팀이 직접 제작한 task에서 사용하고 있다.

### Debugging
Grunt는 평상시에는 에러 스택을 보여주지 않는다. 디버깅할 때 필요하면 `--stack` 옵션을 사용하자. 이게 귀찮다면, 쉘에 `alias grunt='grunt --stack'`같은 alias를 하나 추가해라. (bash의 경우임)

### Storing task files

데이터 파일은 프로젝트 루트 폴더를 기준으로 .grunt/[npm-module-name]/  폴더에만 저장하자. 그리고 적절할 때 알아서 지워라. 임시 파일을 저장하고 싶을 때는 이렇게 하면 안 되고 OS 수준의 임시 폴더 장점을 활용하는 npm 모듈([temporary](https://npmjs.org/package/temporary)이나 [tmp](https://npmjs.org/package/tmp)같은)을 사용해라.

### Avoid Changing the Current Working Directory: `process.cwd()`
다른 이슈가 없다면, 현재 작업 폴더가 gruntfile 파일이 있는 폴더로 설정된다. 사용자는 자기 gruntfile에서 `grunt.fle.setBase()`를 사용해서 이를 변경할 수 있지만, 플러그인은 이게 바뀌면 안된다.

`path.resolve('foo')` can be used to get the absolute path of the filepath 'foo' relative to the `Gruntfile`.

`path.resolve('foo')`라고 하면 Gruntfile 기준의 상대경로 'foo'의 절대경로를 쓸 수 있다.
