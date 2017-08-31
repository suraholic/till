# Express  
* Node.js 생태계에서 가장 널리 쓰이는 웹 프레임워크  
* 내장하고 있는 기능은 매우 적으나, 미들웨어를 주입하는 방식으로 기능을 확장하는 생태계를 가지고 있음   
[공식 매뉴얼 한국어 번역 ](https://expressjs.com/ko/)  

## Express 앱의 기본 구조  
```
// Express 인스턴스 생성
const app = express()

// 미들웨어 주입
app.use(sessionMiddleware())
app.use(authenticationMiddleware())

// 라우트 핸들러 등록
app.get('/', (request, response) => {
  response.send('Hello express!')
})

// 서버 구동
app.listen(3000, () => {
  console.log('Example app listening on port 3000!')
})
```  
> app.use 전체에서 미들웨어 사용 
## Routing 
```
// HTTP 요청 메소드(GET, POST, ...)와 같은 이름의 메소드를 사용
app.get('/articles', (req, res) => {
  res.send('Hello Routing!')
})
// 특정 경로에만 미들웨어를 주입하는 것도 가능
app.post('/articles', bodyParserMiddleware(), (req, res) => {
  database.articles.create(req.body)
    .then(() => {
      res.send({ok: true})
    })
})
// 경로의 특정 부분을 함수의 인자처럼 입력받을 수 있음
app.get('/articles/:id', (req, res) => {
  database.articles.find(req.params.id) // `req.params`에 저장됨
    .then(article => {
      res.send(article)
    })
})
```  
> bodyParserMiddleware() 특정경로에서만 미들웨어 사용  
> res.send에 문자열은 html로 객체를 넣으면 json으로 간주됨  
> /articles/:id = req.params.id   
> res.send에 숫자는 보낼 수 없음 (텍스트&json)

## Request 객체  
* req.body 요청 바디를 적절한 형태의 자바스크립트 객체로 변환하여 저장 (body-parser 미들웨어에 의해 처리됨)  
* req.ip 요청한 쪽의 IP  
* req.params  route parameter  
* req.query  querystring이 객체로 저장됨  

## Response 객처  
* res.status(...) 응답의 상태 코드를 지정하는 메소드 
* res.append(...) 응답의 헤더를 지정하는 메소드  
* res.send(...) 응답의 바디를 지정하는 메소드, 인자가 텍스트면 text/html, 객체면 application/json 타입으로 응답  

# Template Language 
- Static Web Page
  누가 어떻게 요청하든 미리 저장되어 있던 HTML 파일을 그대로 응답  
- Dynamic Web Page 
  요창한 사람과 요청한 내용에 따라 각각 다른 내용으로 편집한 HTML을 응답  

## Template Engine  
* 템플릿과 데이터를 결합해 문서를 생성하는 프로그램, 혹은 라이브러리  
* 템플릿을 작성할때 사용하는 언어를 템플릿 언어라고 함  
### EJS (Embedded Javascript Template)  
* Node.js 생태계에서 가장 많이 사용되는 템플릿 엔진 
* 문법이 단순 
* Javascript 코드를 템플릿 안에서 그대로 쓸 수 있음 
  > ejx 확장팩을 vscode에 설치  

[ejs 공홈](http://ejs.co/) 

```
<%# index.ejs %> -> ejx 주석
<html>
  <head>
    <title><%= title %></title>
  </head>
  <body>
    <div class="message">
      <%= message %>
    </div>
    <% if (showSecret) { %>
      <div>my secret</div>
    <% } %>
  </body>
</html>
```
## Express에서 EJS 사용하기  
* ejs 설치 
```
$ npm install --save ejs
```
* template engin 설정 
```
app.set('view engine', 'ejs')
```

* res.render()  : express response 메소드 
```
// index.ejs 파일 
const data = {
  title: 'Template Language',
  message: 'Hello EJS!',
  showSecret: true
}
res.render('index.ejs', data)  
```  
* ejs 연동 example (server.js)  
```
const express = require('express')

const app = express()

// 템플릿 엔진을 ejs로 설정해줍니다.
app.set('view engine', 'ejs')

app.get('/', (req, res) => {
  // 템플릿에서 사용할 데이터입니다.
  // 배열에 요소를 추가하고, true를 false로 바꾸고, 텍스트를 변경해보세요
  const data = {
    items: ['one', 'two', 'three'],
    showSecret: true,
    secret: 'I LOVE NODE.JS!',
    name: req.query.name
  }
  // res.render 함수는 views 디렉토리에 있는 템플릿 파일과 데이터를 합쳐서 응답을 보냅니다.
  res.render('index.ejs', data)
})

app.listen(3000, function() {
  console.log('listening...')
})
```
* index.ejs example  
```
<%# 주석입니다 %> 
<!DOCTYPE html>
<html>
  <head>
  </head>
  <body>
    <% if (name) { %>
      <p>당신의 이름은 <%= name %>입니다.</p>
    <% } else { %>
      <p>이름이 주어지지 않았습니다. query parameter에 name을 추가해보세요.</p>
    <% } %>
    <hr>
    <h1>List</h1>
    <ul>
      <% items.forEach(item => { %>
        <li><%= item %></li>
      <% }) %>
    </ul>
    <% if (showSecret) { %>
      <p>my secret is: <%= secret %></p>
    <% } %>
  </body>
</html>
``` 
> ejs 주석은 렌더링시 삭제된다. 

## Serving Static Files  
```
// `public` 폴더에 있는 파일을 `/static` 경로 아래에서 제공
app.use('/static', express.static('public'))  

<!-- 템플릿 파일에서 참조할 수 있음 -->
<link rel="stylesheet" href="/static/index.css">
<script type="text/javascript" src="/static/index.js"></script>
```
> express.static

#### 다른 템플릿 언어 [pug](https://pugjs.org) 















