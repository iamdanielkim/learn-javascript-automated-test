# 테스트 w/ Protractor


Protractor 기반의 테스트를 작성한다는 것은 실재로는 selenium-webdriver 로 테스트를 작성하는 것과 같다. Protractor는 selenium-webdriver를 좀더 쉽게 작성할 수 있도록 helper를 제공한다.


## Webdriver



## 테스트 수행하기

### 테스트 케이스 작성

* test/spec/sample.coffee

```coffee
describe 'angularjs homepage', () ->
  it 'should have a title', () ->
    browser.get('http://juliemr.github.io/protractor-demo/')
    expect(browser.getTitle()).toEqual('Super Calculator')
```

### 실행

```$>protractor conf.js```












