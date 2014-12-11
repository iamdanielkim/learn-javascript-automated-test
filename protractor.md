## protractor 설치하기


### 설치

```
npm install -g protractor
```

설치가 완료되면 ```protractor```, ```webdriver-manager``` 두 CLI 도구가 설치된다.

 ```protractor```는 Testrunner, ```webdriver-manager```는 selenium server를 손쉽게 구동시켜주는 helper역할을한다.

#### selenium download
```
webdriver-manager update
```

#### selenium start
```
webdriver-manager start
```

위와 같이 실행을 하면 ```http://localhost:4444/wd/hub``` 에서 서버가 뜬 것을 확인할 수 있다.



### 설정

설치가 완료되면 추가적인 설정이 필요하다. 간단하게 아래와 같이 작성하면 설정이 완료된다.

* conf.js

```
exports.config = {
  seleniumAddress: 'http://localhost:4444/wd/hub',
  specs: ['test/spec/**/*.coffee']
}

```

//advanced 설정 내용
