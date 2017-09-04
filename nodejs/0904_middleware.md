# Middleware  

```
// 미들웨어 = 함수
function helloMiddleware(res, req, next) {
  console.log('hello')
  next()
}

app.use(helloMiddleware)
```

* 미들웨어는 함수다, 즉, 어떤 작업이든 가능하다  
* request 객체, response 객체, next 함수를 인자로 받는다  
* request, response 객체를 조작해서 기능을 구현 한다  
* 다음 미들웨어를 동작시키기 위해 next함수를 인자 없이 호출한다  
* 미들웨어는 등록된 순서되로 실행 되므로, 등록 순서가 중요하다  
```
미들웨어는 req, res에 더해서 next라는 함수를 추가로 인자로 받는다. next 함수를 호출하면 다음 미들웨어로 처리를 넘기는 효과가 있다. 만약에 미들웨어가 next 함수를 호출하지도 않고, 응답도 보내지 않으면 클라이언트는 응답을 받지 못하게 되므로 주의! 꼭 next를 호출하자!! 

* next() 호출하거나 res.send('')를 해야한다
```

* 미들웨어를 앱 전체에서 동작하도록  
  > app.use(heeloMiddleware)  
* 특정 경로에서만 동작하도록 
  > app.use('/some-path', helloMiddleware)
* 한번에 여러개 동작하도록  
  > app.use(middleware1, middleware2 ...)  
## 미들웨어로 하는 일 
  * 로킹 (morgan) 
  * Http body를 객체로 변환  
  * 사용자 인증  
  * 권한 관리 
  * ... 
## 미들웨어를 왜 사용할까?
미들웨어로 할 수 있는 모든 일은 라우트 핸들러에서도 할 수 있으나, 여러 라우터에서 사용해야하는 기능을 중복 작성하는 불편을 덜고 코드를 재사용 하기 위해 미들웨어를 사용하는 것

* middlewares.js   

```
exports.ipLoggingMiddleware = (req, res, next) => {
  console.log(`request ip: ${req.ip}`)
  next()
}

exports.urlLoggingMiddleware = (req, res, next) => {
  console.log(`request url: ${req.originalUrl}`)
  next()
}

exports.resLocalMiddleware = (req, res, next) => {
  res.locals.myVar = 'FASTCAMPUS!'
  next()
}

exports.lock = key => (req, res, next) => {
  if (req.query.key === key) {
    next()
  } else {
    res.status(403)
    res.send('403 Forbidden')
  }
}
```

* server.js  
```
const express = require('express')
const {
  ipLoggingMiddleware, 
  urlLoggingMiddleware, 
  resLocalMiddleware,
  lock
} = require('./middlewares')

const app = express()

app.set('view engine', 'ejs')

app.use(urlLoggingMiddleware)
app.use(ipLoggingMiddleware)

app.get('/', resLocalMiddleware, (req, res) => {
  const data = {}
  res.render('index.ejs', data)
})

app.get('/secret', lock('thisisthekey'), (req, res) => {
  res.send('my secret is...')
})

//요청이 라우터 핸들러가 등록된 어떤 경로와도 일치하지 않을 때 404페이지 실행 ( 맨마지막 미들웨어를 실행시킨다)

app.use((req, res, next) => {
  res.render('404.ejs')
})
```

* 라우터핸들러와 일치하지 않으면 다음으로 건너 뛴다. 
* 마지막에 라우터 처리해야한다 



```
> 커링 함수
makeAdder2 = x => y => x+y 
makeAdder2(4)(5)

function makeAdder(x){
	return function(y){
		return x+y
    }
}
add1 = makeAdder(1)
add1(2) 또는 makeAdder(3)(4) 
```
# 미들웨어 vs 라우트 핸들러  
* 라우트 핸들러도 미들웨어
* 즉, next 함수를 인자로 받는 것이 가능 

``` 
app.get('/', (req, res, next) => {
  if (!someCondition) {
    next() // 요청을 처리를 하지 않고 다른 핸들러로 넘김
  } else {
    res.send('hello')
  }
})
```

# 에러 처리 미들웨어 

* 다른 미들웨어 함수와 동일반 방법으로 오류 처리 미들웨어 함수를 정의할 수 있지만, 오류 처리 함수는 3개가 아닌 4개의 인수, 즉 (err, req, res, next)를 갖는다는 점이 다르다  

* 오류 처리 미들웨어는 다른 app.use() 및 라우트 호출을 정의한 후에 마지막으로 정의해야 한다  
```
app.use(function(err, req, res, next) {
  console.error(err.stack);
  res.status(500).send('Something broke!');
});
```


