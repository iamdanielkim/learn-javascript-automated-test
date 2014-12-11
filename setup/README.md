# 단위 테스트 w/ karma


## Karma 설치하기



karma를 통해 단위 테스트환경을 구성하고 테스트를 자동화하는 방법에 대해 알아본다.

### karma 설치


```
  npm install karma --save-dev
  npm install -g karma-cli

  npm install karma-chrome-launcher --save-dev


```

### 설정

#### 프로젝트 구조

아래와 같은 형태로 프로젝트 구조를 생성한다.
```
ROOT/
    fixture/
    assets/
        js/
        stylesheets/
    test/
        unit/
    html/
    dist/
    bower.json
    package.json
```

이후 ```karma init``` 명령을 통해 기본 karma.conf.js를 생성한다.

#### mocha, chai 사용하기

karma-mocha karma-chai 를 설치 후 frameworks에 mocha, chai를 설정한다.

```
  # test framework으로 mocha, chai 사용
  npm install karma-mocha karma-chai --save-dev
```

```
  frameworks: ['mocha','chai']
```

#### coffeescript 사용하기

karma에서 coffeescript를 사용하기 위해서
karma-coffee-preprocessor를 설치 후 karma.conf.js에 preprocessor로 등록한다.

```
  # karma-coffee-preprocessor 설치
  npm install karma-coffee-preprocessor --save-dev
```

```
    preprocessors: {
      '**/*.coffee': ['coffee'],
    }
```
#### fixture 관리하기

###### html fixture 사용하기

karma에서 html fixture를 사용하기 위해서
karma-html2js-preprocessor를 설치 후 karma.conf.js에 preprocessor로 등록한다.

```
  # karma-html2js-preprocessor 설치
  npm install karma-html2js-preprocessor --save-dev
```

```
    preprocessors: {
      'fixture/**/*.html': ['html2js']
    }
```

### 테스트 실행하기

설정이 완료되면 ```karma start karma.conf.js```으로 테스트를 수행할 수 있다.



### 전체 karma.conf.js

```javascript
module.exports = function(config) {
  config.set({

    // base path that will be used to resolve all patterns (eg. files, exclude)
    basePath: '',


    // frameworks to use
    // available frameworks: https://npmjs.org/browse/keyword/karma-adapter
    frameworks: ['mocha','chai'],


    // list of files / patterns to load in the browser
    files: [
      'components/jquery/dist/jquery.min.js',
      'fixture/**/*.html',
      'assets/**/*.*',
      'test/**/*.*'
    ],


    // list of files to exclude
    exclude: [
    ],


    // preprocess matching files before serving them to the browser
    // available preprocessors: https://npmjs.org/browse/keyword/karma-preprocessor
    preprocessors: {
      '**/*.coffee': ['coffee'],
      'fixture/**/*.html': ['html2js']
    },

    // test results reporter to use
    // possible values: 'dots', 'progress'
    // available reporters: https://npmjs.org/browse/keyword/karma-reporter
    reporters: ['progress'],


    // web server port
    port: 9876,


    // enable / disable colors in the output (reporters and logs)
    colors: true,


    // level of logging
    // possible values: config.LOG_DISABLE || config.LOG_ERROR || config.LOG_WARN || config.LOG_INFO || config.LOG_DEBUG
    logLevel: config.LOG_INFO,


    // enable / disable watching file and executing tests whenever any file changes
    autoWatch: true,


    // start these browsers
    // available browser launchers: https://npmjs.org/browse/keyword/karma-launcher
    browsers: ['Chrome'],


    // Continuous Integration mode
    // if true, Karma captures browsers, runs the tests and exits
    singleRun: false
  });
};
```

## 커버리지 측정 w/ Istanbul


### 설치하기

karma환경에서 istanbul로 커버리지를 측정하기 위해
karma-coverage 플러그인을 설치한다.

```
 npm install karma-coverage --save-dev
```

### 설정

reporters, preprocessors에 coverage 설정을 추가한다.

### coverage 설정이 추가된 karma.conf.js
```javascript
// karma.conf.js
module.exports = function(config) {
  config.set({

    .....

    // coverage reporter generates the coverage
    reporters: ['progress', 'coverage'],

    preprocessors: {
      // source files, that you wanna generate coverage for
      // do not include tests or libraries
      // (these files will be instrumented by Istanbul)
      'src/*.js': ['coverage']

      // source files, that you wanna generate coverage for
      // do not include tests or libraries
      // (these files will be instrumented by Istanbul via Ibrik unless
      // specified otherwise in coverageReporter.instrumenter)
      'src/*.coffee': ['coverage'],

      // note: project files will already be converted to
      // JavaScript via coverage preprocessor.
      // Thus, you'll have to limit the CoffeeScript preprocessor
      // to uncovered files.
      'test/**/*.coffee': ['coffee']

    },

    // optionally, configure the reporter
    coverageReporter: {
      type : 'html',
      dir : 'coverage/'
    }
  });
};

```

### 실행하기

동일하게 ```karma start karma.conf.js``` 명령을 통해 테스트를 수행 하면서 동시에 커버리지 정보 역시 확인 할 수 있다.



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










