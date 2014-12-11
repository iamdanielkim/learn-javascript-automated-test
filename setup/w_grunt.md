# Grunt 설치하기

javascript 빌드도구로 많이 사용되는 grunt를 통해 빌드 시 필요한 기본적인 task를 작성하고 실행하는 방법에 대해 알아본다.

## 설치

```
npm install -g grunt-cli
npm install grunt --save-dev
```

### plugin 설치

grunt 공식 plugin은 grunt-contrib- 의 형태로 이름이 되어있다.

```
## 모든 plugin을 설치할 경우
npm install grunt-contrib --save-dev

## 특정 plugin만 선택하여 설치할 경우
npm install grunt-contrib-uglify --save-dev
npm install grunt-contrib-jshint --save-dev
npm install grunt-contrib-watch --save-dev
npm install grunt-contrib-concat --save-dev

```

## Gruntfile.js 설정

grunt에서 제공하는 [Sample-Gruntfile] 문서의 소스를 참고하여 생성한다.

## 빌드 실행


```
 # default task 실행
$> grunt

 # 'watch' task 실행
$> grunt watch

```

## 전체 Gruntfile.js 예시

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
      tasks: ['jshint']
    }
  });

  grunt.loadNpmTasks('grunt-contrib-uglify');
  grunt.loadNpmTasks('grunt-contrib-jshint');
  grunt.loadNpmTasks('grunt-contrib-watch');
  grunt.loadNpmTasks('grunt-contrib-concat');

  grunt.registerTask('test', ['jshint']);

  grunt.registerTask('default', ['jshint', 'concat', 'uglify']);

};

```



[Sample-Gruntfile]: http://gruntjs.com/sample-gruntfile
