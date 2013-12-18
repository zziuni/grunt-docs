커멘드 명령어 `grunt`는 몇 가지 옵션을 가지고 있다. 이 옵션들을 보고 싶으면 터미널에 `grunt -h`라고 입력해라.

### --help, -h
도움말 출력.

### --base, -b
기본 경로를 다른 곳으로 지정한다. 기본적으로 모든 파일 경로는 `Gruntfile`위치를 기준으로 한다.

`grunt.file.setBase(...)`와 같다.

### --no-color
터미널 출력에서 색깔을 제거한다.

### --gruntfile
다른 `Gruntfile`을 지정한다.

grunt는 원래 현재 폴더나 가장 가까운 상위 폴더에서 `Gruntfile.js`, `Gruntfile.coffe`를 찾는다.

### --debug, -d
이 옵션을 task가 지원할 경우 디버깅 모드를 활성화한다.

### --stack
warning이나 fatal error 발생시 스택 트래이스를 출력한다.

### --force, -f
warning을 무시하고 실행할 수 있다.

조언하자면... 코드를 고쳐라. 이 옵션을 사용하지 말고.

### --tasks
task와 추가 파일을 탐색할 폴더 경로를 추가한다.

`grunt.loadTasks(...)`와 같다.

### --npm
task와 추가 파일을 탐색할 이미 설치된 npm grunt plugin

`grunt.loadNpmTasks(...)`와 같다.

### --no-write
파일 작성을 금한다.

### --verbose, -v

더 많은 정보가 출력되는 상세모드

### --version, -V
grunt 버전을 출력한다. --verbose를 같이 쓰면 정보가 더 나온다.

### --completion
자동 완성 규칙을 쉘에 출력한다. 자세한 정보는 grunt-cli 문서를 참조해라.
