# build


## Grunt 설치하기

javascript 빌드도구로 많이 사용되는 grunt를 통해 빌드 시 필요한 기본적인 task를 작성하고 실행하는 방법에 대해 알아본다.

### 설치

```
npm install -g grunt-cli
npm install grunt --save-dev
```

#### plugin 설치

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

### Gruntfile.js 설정

grunt에서 제공하는 [Sample-Gruntfile] 문서의 소스를 참고하여 생성한다.

### 빌드 실행


```
 # default task 실행
$> grunt

 # 'watch' task 실행
$> grunt watch

```

### 전체 Gruntfile.js 예시

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

## grunt usemin을 사용한 환경 별 JS 로딩 변경


### usemin 플러그인 설치

```bash
npm install grunt-usemin --save-dev
npm install grunt-contrib-concat --save-dev
npm install grunt-contrib-cssmin --save-dev
npm install grun-contrib-uglify --save-dev

```

### Gruntfile.js 설정

#### plugin 로딩

```javascript
grunt.loadNpmTasks('grunt-usemin');
grunt.loadNpmTasks('grunt-contrib-concat');
grunt.loadNpmTasks('grunt-contrib-cssmin');
grunt.loadNpmTasks('grunt-contrib-uglify');

```

#### task 등록

```javascript
grunt.registerTask('build', [
    'useminPrepare',
    'concat:generated',
    'cssmin:generated',
    'uglify:generated',
    'usemin'
]);

```

#### task 설정

###### 프로젝트 구조
```
ROOT/
    fixture/
    assets/
        js/
        stylesheets/
    test/
        unit/
    html/
        livelist.html
    dist/
    bower.json
    package.json
```

###### livelist.html

```html

    <!-- build:js /assets/js/livelist-all.js -->
    <script src="/assets/js/item.js"></script>
    <script src="/assets/js/livelist_serializer.js"></script>
    <script src="/assets/js/livelist.js"></script>
    <!-- endbuild -->

    ...
```

```javascript
grunt.initConfig({
    useminPrepare: {
      src: ['html/livelist.html'],
      options: {
        root: './',
        dest: 'dist'
      }
    },
    usemin: {
      html: 'html/livelist.html'
    }
});

```

### 실행

```$> grunt build```

[grunt-usemin]: https://www.npmjs.org/package/grunt-usemin







[Sample-Gruntfile]: http://gruntjs.com/sample-gruntfile
