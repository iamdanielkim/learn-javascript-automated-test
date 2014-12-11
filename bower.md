# bower


설치
```
### bower 설치
npm install bower -g

### component 설치
bower install <package-name>

```

### bower 설정하기

#### 환경설정

.bowerrc
```
{
  "directory": "components"
}
```


```bower init``` 명령으로 bower.json의 skeleton을 만들 수 있다.

bower.json
```
{
  "name": "livelist",
  "version": "0.1.0",
  "homepage": "https://github.com/iamdanielkim/livelist",
  "authors": [
    "iamdanielkim <iamdanielkim@sk.com>"
  ],
  "description": "wysiwyg list",
  "main": "lib/index.js",
  "keywords": [],
  "license": "MIT",
  "ignore": [
    "**/.*",
    "node_modules",
    "components",
    "test"
  ],
  "devDependencies": {
    "mocha": "~1.18.2",
    "assert": "~0.0.2",
    "coffee-script": "~1.7.1",
    "chai": "~1.9.1"
  },
  "dependencies": {
    "jquery": "~2.1.1"
  }
}
```

#### 의존성 관리하기
```
bower install <component> --save
bower install <component> --save-dev
```

### bower 컴포넌트로 등록하기

```
bower register <컴포넌트 이름> <git 저장소 주소>

```

예시
```
bower register livelist git@github.com:iamdanielkim/livelist.git
```










