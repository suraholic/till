# session 
* HTTP Session 
  요청해서 응답까지 걸리는 시간 
* 로그인 세션 
  로그인-로그아웃까지의 시간 
* Google Analytics 세션  
  페이지 접속 - 30분간 접속이 없으면 종료로 간주 (커스터마이징 가능)  

# 웹 서비스를 위한 세션의 구현 
1. 세션이 시작되면, 세션이 시작되었다는 사실을(ex: 로그인) 쿠키에 저장  
2. 세션에 대한 정보를 여러 요청에 걸쳐서 지속시키기 위해, 정보를 어딘가에 저장 
3. 세션이 만료되면(ex: 로그아웃), 세션이 만료되었다는 사실을 쿠키에 반영 
  > 일반적으로 사용되는 방식 임

# 세션 스토어
세션에 대한 정보를 저장하는 어딘가
* 쿠키 : 정보의 형태가 간단하고 자주 바뀔 일이 없으면   
* 데이터베이스 : 저장해야 할 정보의 양이 많으면 
* 파일
* 기타 저보를 저장할 수 있는 곳 어디든
  > 정보가 굉장히 자주 변경되면 메모리 기반 저장소  

### '세션'과 '세션 스토어'는 엄연히 다른말 이지만 혼용되는 경우가 많다. 
### '세션에 정보를 저장한다'는 말은 '세션 스토어에 정보를 저장한다'는 말과 같은 뜻이다. 

# Express + Session 
* cookie-session 
  쿠키에 모든 정보를 저장하는 세션 스토어. 첫 방문시 무조건 세션 시작 
* express-session 
  쿠키에는 세션 식별자만 저장하고 실제 정보의 저장은 외부 저장소(데이터베이스 등)를 이용하는 세션 스토어, 외부 저장소에 대한 별도의 설정 필요  

## cookie-session 예제 
cookie-session NPM 패키지는 쿠키를 세션 스토어로 사용할 수 있도록 해주며 세션 스토어를 쉽게 사용할 수 있는 방법을 제공  

## cookie-session 동작 방식 
1. cookie-session 미들웨어는 첫 요청이 일어났을 때 빈 세션 정보(빈 객체)를 req.session에 주입  
2. 프로그래머는 세션과 관련된 정보를 req.session에 저장합니다. req.session은 보통의 자바스크립트 객체로, JSON으로 표현될 수 있는 자료라면 뭐든지 저장할 수 있다.
3. cookie-session 미들웨어는 응답이 일어나기 직전에 req.session 객체를 문자열로 바꾼 뒤(JSON & base64), 쿠키에 저장 
4. cookie-session 미들웨어는 다음 번 요청부터 쿠키에 저장된 정보를 자바스크립트 객체로 변환해 req.session에 주입
5. 프로그래머는 req.session 객체를 이용해 세션 정보를 읽을 수 있습니다. 또한 세션 정보를 통째로 삭제하기 위해 미들웨어 또는 라우트 핸들러에서 req.session = null을 대입할 수 있다. 
```
  const express = require('express')
  const cookieSession = require('cookie-session')

  const app = express()

  app.set('trust proxy', 1) 
  app.set('view engine', 'ejs')

  // cookie-session 설정
  // name: 쿠키 이름으로 사용할 문자열
  // secret: 세션 정보를 서명할 때 사용할 키
  app.use(cookieSession({
    name: 'session',
    secret: process.env.SECRET
  }))

  // req.session.count를 처리하는 미들웨어
  const countMiddleware = (req, res, next) => {
    if ('count' in req.session) {
      // count 속성이 있으면 1으.
      req.session.count += 1
    } else {
      // count 속성이 없으면 처음 방문한 것이므로 1로 설정한다.
      req.session.count = 1
    }
    next()
  }

  // 첫 방문 후, 쿠키 관련 헤더가 요청과 응답에 잘 포함되는지 살펴보고,
  // 실제로 쿠키가 어떻게 저장되어있는지 살펴보세요.
  app.get('/', countMiddleware, (req, res) => {
    res.render('index.ejs', {count: req.session.count})
  })

  app.post('/reset-count', (req, res) => {
    delete req.session.count
    res.redirect('/')
  })

  app.listen(3000, function() {
    console.log('listening...')
  })

``` 

 >  cookieSession미들웨어 설정 시 option으로 maxAge 등 설정 할 수 있다 

 # 인증(Authentication)과 인가(Authorization)  

* 인증(Authentication)은 클라이언트가 누구인지를 확인하는 과정 (로그인)

* 인가는 클라이언트가 하려고 하는 작업이 해당 클라이언트에게 허가된 작업인지를 확인하는 과정 (권한설정)(ex: 카페관리자만 게시판 생성가능) 


## 해시함수 
임의의 길이의 데이터를 고정된 길이의 데이터로 매핑하는 함수 
  * 결과가 짧으면 좋다
  * 해시 충돌 확률이 적어야 한다 
    (해시 충돌을 최소화)  

  > ex) md5

*** 
session  
session.slg : 해시
해시 + 비밀키 => 서명 

* 서명을 통한 보안
  - 조작 방지는 가능하지만 데이터 공개는 안된다.
  - 데이터 공개는 암호화를 해야 막아진다 

  

  
