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

## CSS 프로퍼티 값을 지정할 때 사용하는 키워드, 단위, 색상  

#### 키워드  
   각 프로퍼티에 따라 사용할 수 있는 키워드가 (속성 값) 존재한다.   
   > display { block, inline, inline-block, none }   


#### 크기 단위   
   * px : 절대 값으로 화소(px) 단위 이다. (1px은 화소 1개 크기)  
     > 모니터 해상도 1680 * 1050 => 1680px * 1050px   
     > 픽셀은 디바이스 해상도에 따라 상대적 크기를 갖는다  

   * % (백분율) : 요소에 지정된 사이즈(상속된 사이즈나 디폴트 사이즈)에 상대적인 사이즈를 설정  

   * em : 요소에 지정된 사이즈(상속된 사이즈나 디폴트 사이즈)에 배수(倍數) 단위로 상대적인 사이즈를 설정  
     > 2em 은 지정된 사이즈의 2배  
  
   * rem :  최상위 요소(html)의 사이즈를 기준으로 한다.  
    rem의 r은 root를 의미한다.  
  
   * Viewport 단위(vh, vw, vmin, vmax)   
    상대적인 단위로 viewport를 기준으로 한 상대적 사이즈를 의미  
     > IE 8 이하는 지원하지 않으며 IE 9 ~ 11, Edge는 지원이 완전하지 않다.
 
  ~~~
  <style>
    body {
      font-size: 14px;
      text-align: center;
    }
    div {
      font-size: 1.2em; /* 14px * 1.2 = 16.8px */
      font-weight: bold;
      padding: 2em;
    }
  </style>  
  
   <body>
      <div class='box1'>
        Font size: 1.2em ⇒ 14px * 1.2 = 16.8px
        
        <div class='box2'>
          Font size: 1.2em ⇒ 16.8px * 1.2 = 20.16px
          
          <div class='box3'>
            Font size: 1.2em ⇒ 20.16px * 1.2 = 24.192px
          </div>
        
        </div>
      </div>
    </body>
   ~~~
  
  > 중첩된 자식 요소에 em을 지정하면 모든 자식 요소의 사이즈에 영향을 미친다.   
  > 즉 상황에 따라 1.2em은 각기 다른 값을 가질 수 있다.

  

  > 폰트 기본 사이즈는 16px 이다.  
  > Reset CSS를 사용하여 html 요소의 font-size 지정 필요 

#### 색상 표현 단위  

* HEX 코드 단위 (Hexadecimal Colors)	: #000000  
* RGB (Red, Green, Blue)	: rgb(255, 255, 0)  
* RGBA (Red, Green, Blue, Alpha/투명도)	: rgba(255, 255, 0, 1)  
* HSL (Hue/색상, Saturation/채도, Lightness/명도)	: hsl(0, 100%, 25%)  
* HSLA (Hue, Saturation, Lightness, Alpha)	: hsla(60, 100%, 50%, 1)  

~~~
<style>
      #hex-p1 {background-color:#ff0000;}      
      #rgb-p1 {background-color:rgb(255,0,0);}      
      #rgba-p1 {background-color:rgba(255,0,0,0.3);
</style>
~~~  

*** 

#### 웹폰트 (Web Font)  
사용자 (클라이언트 PC) 에게 css에 정의한 폰트가 없다면 서버에서 전송받아 폰트를 적용  

* CDN(Content Delivery Network) 링크 방식  
~~~
  /* google 웹폰트 사용하는 방법 */

  @import url(http://fonts.googleapis.com/earlyaccess/nanumgothic.css);

* { font-family: 'Nanum Gothic', sans-serif; }
~~~  

* 서버 폰트 로딩 방식  

~~~
  @font-face {
    font-family:"Nanum Gothic";
    src:url("NanumGothic.eot"); /* IE 9 호환성 보기 모드 대응 */
    src:local("☺"),             /* local font 사용방지, 생략 가능 */
        url("NanumGothic.eot?#iefix") format('embedded-opentype'), /* IE 6~8 */
        url("NanumGothic.woff") format('woff'); /* 표준 브라우저 */
  }

  * { font-family: "Nanum Gothic", sans-serif; }
~~~
> 폰트 파일을 서버에 두고 요청이 오면 클라이언트로 전송하는 방식  
>  브라우저에 따라 지원하는 폰트 파일 형식이 다르기 때문에 파일 형식을 고려해야 한다.  

> 영문과 한글을 따로 지정 할 때는 영문 폰트 먼저, 그 다음 한글 폰트 지정 한다.    
> 한글 폰트부터 지정하면 영문에도 한글 폰트가 지정된다.

 




  

 
  


  