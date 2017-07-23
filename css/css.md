![Alt text](./images/css-syntax.png)  

* 셀렉터 (Selector, 선택자)  
  스타일을 적용하고자 하는 HTML 요소를 선택하기 위해 CSS에서 제공하는 수단  

* 프로퍼티 (Property / 속성)  
  표준 스펙으로 이미 지정되어 있어 사용자가 임의로 정의할 수 없다  
  세미콜론(;)으로 구분하여 여러개의 프로퍼티를 지정할 수 있다  

* 값 (Value / 속성값)  
  프로퍼티의 값으로, 각 프로퍼티마다 갖는 값이 다르다 (크기, 단위, 색상 등)  

***

## HTML에서 CSS 연동  
  1. HTML 내부에 CSS 포함 (Embedding style)  
  ~~~
  <!DOCTYPE html>
  <html>
    <head>
      <style>
        h1 { color: red; }
        p  { background: aqua; }
      </style>
    </head>
  </html>
  ~~~  

  2. HTML 외부 파일을 Link로 연동  
  ~~~
  <!DOCTYPE html>
  <html>
    <head>
      <link rel="stylesheet" href="css/style.css">
    </head>
  </html>
  ~~~  

  3. HTML 태그에 직접 사용 (Inline style)  
  ~~~
  <h1 style="color: red"> 제목 </h1>
  <p style="background: aqua"> 내용 </p>
  ~~~  

> Reset CSS (초기화) :   
  브라우저마다 가지고 있는 기본 CSS를 하나의 스타일로 통일한다  

***



 
  


  