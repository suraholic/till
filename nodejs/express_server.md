# Express Server.js 설정  

```
  npm install --save express
  npm install --save pug
```

* express & 템플릿 pug (ajde) 연결  
  ```
  const express = require('express')  
  const pug     = require('pug')  
  const app = express()  
  ```

*  사용할 view engine을 express에게 알려주는 코드  
*  express 프레임워크와 pug (jade) 엔진을 연결!  
   ```
    app.set('view engine', 'jade')  
   ```

* 템플릿이 있는 디렉토리를 express에게 알려주는 코드   
* (생략가능, 디폴트 경로 ./views) 
* views 폴더안에 pug 파일을 넣는다   
  ```
    app.set('views', './views')  
  ```

* 이미지, CSS 파일 및 JavaScript 파일과 같은 정적 파일 제공  
* Express의 기본 제공 미들웨어 함수인 express.static을 사용  
* 정적 디렉토리에 대해 상대적으로 파일을 검색하며, 따라서 정적 디렉토리의 이름은 URL의 일부가 아니다  
* http://localhost:3000/static/css/style.css  
```
  app.use('/static', express.static('public'))  
```

* 라우팅 설정  : URI(또는 경로) 및 특정한 HTTP 요청 메소드(GET, POST 등)인 특정 엔드포인트에 대한 클라이언트 요청에 응답하는 방법을 
결정하는 것    
  ```
  app.get('/', (req,res) => {
    res.send("OK");
  });

  app.get('/', function (req, res) {
    res.send('Hello World!');
  });

  app.put('/user', function (req, res) {
    res.send('Got a PUT request at /user');
  });
  ``` 
* 템플릿 엔진이 적용된 페이지의 라우터 설정  
  ```
  app.get('/', (req, res)=>{
    res.render('index');
  })
  ```
* express server 구동  
  ```
  app.listen(3000, ()=>{
    console.log('server start...')
  })
  ```

  ## server.js & pug 간단한 설정
  ```
  const express = require('express')
  const pug     = require('pug')

  const app = express()

  app.set('view engine', 'pug')
  app.set('views', './views');
  app.locals.basedir = app.get('views');
  app.use('/static', express.static('/public'))

  app.get('/', (req,res) => {
    res.render('index')
  })

  app.listen(3000, ()=>{
    console.log('Connected to 3000 port...')
  })

```

