## 테스트 w/ Karma, Mocha

Karma는 테스트 환경을 제공할 뿐이고, 실제 테스트는  사용하는 test framework에 따라 작성 방법이 차이날 수 있다. 여기에서는 mocha, chai를 기반으로 테스트 작성하는 방법에 대해 설명한다.

### karma로 테스트하기

```karma start karma.conf.js``` 명령을 통해 실행가능하다.


### grunt로 karma 테스트하기

```npm install grunt-karma --save-dev``` 로 plugin 설치한다.

서치 완료 후, 아래와 같이 Gruntfile.js에 karma설정을 추가한다.

```
...
karma: {
  // 공통 설정
  options: {
    configFile: 'karma.conf.js',
    browsers: ['Chrome']
  },
  // CI용 설정
  continuous: {
    singleRun: true,
    browsers: ['PhantomJS']
  },
  // 개발 시 설정
  dev: {
    background: true,
    singleRun: false
  }
}
...

watch: {
  tasks: ['karma:dev:run']
}
...
grunt.loadNpmTasks('grunt-karma');

grunt.registerTask('test', ['karma:dev:start', 'watch']);

```

* ```grunt karma:dev``` - karma test 수행
* ```grunt test``` - 파일 변경 시 마다 test 자동 수행
* ```grunt karma:continuous``` - CI 환경에서 test 수행





