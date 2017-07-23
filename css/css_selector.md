# Style을 적용하기 위해 HTML 요소 선택  
## 셀렉터 지정

1. 전체 셀렉터 :   ` * { color: red; }`  
   HTML 안에 모든 요소에 적용됨 

2. 태그 셀렉터 : `p { color: red; }`  
   HTML 안에 모든 p 태그 요소를 선택  

3. ID 셀렉터 ( # 으로 표시) : `#p1 { color: red; }`  
   태그에 id 속성값을 지정하여 일치되는 요소를 선택
~~~  
  <style>
    /* id 값이 p1인 요소를 선택 */
    #p1 { color: red; }
  </style>
 
 <body>
    <p id="p1"> 선택!! </p>
    <p id="p2"> 선택 안됨 </p>
 </body>
~~~
> id 속성은 html 문서내에서 유일해야 한다.  

4. class 셀렉터 ( . 으로 표시) : `.p1 { color: red; }`  
   태그에 class 속성값을 지정하여 일치되는 요소를 선택  
~~~  
  <style>
    /* class 값이 p1인 요소를 선택 */
    .p1 { color: red; }
  </style>

  <body>
    <p class="p1"> 선택!! </p>
    <p class="p1"> 선택!! </p>
    <p class="p2"> 선택 안됨 </p>
  </body>
~~~  
> class 속성은 html 문서내에서 여러번 사용 가능  

5. 어트리뷰트 셀렉터 : `a[href] ` 
   a 요소 중에 href 속성을 갖는 모든 요소를 선택

6. 셀렉터[어트리뷰트 = ”값”] : `a[target = "_blank"] ` 
   a 요소 중에 target 값이 "_blank"인 모든 요소를 선택  

7. 셀렉터[어트리뷰트 ~= ”값”] : `h1[title ~= "first"]`  
~~~
  <style>
    /* h1 중에 title 값에 (공백으로 분리된) "first" 단어가 포함되는 요소 */
    h1[title~="first"] { color: red; }
  </style>

  <body>
    <h1 title="heading first"> 선택!! </h1>
    <h1 title="heading-first"> 선택 안됨 </h1>
  </body>
~~~

8. 셀렉터[어트리뷰트 |= ”값”] : ` p[lang |= "en"]`  
   지정된 속성 값과 일치하거나 "값-" 으로 시작되는 모든 요소
~~~ 
  <style>
    /* p 중에 lang 값이 "en"과 일치하거나 "en-"로 시작 */
    p[lang|="en"] { color: red; }
  </style>

  <body>
    <p lang="en"> 선택!! </p>
    <p lang="en-us">  선택!!  </p>
    <p lang="en-gb"> 선택 안됨  </p>
  </body>
~~~

9. 셀렉터[어트리뷰트 ^= ”값”] : `a[href ^= "https://"] `  
   지정된 속성 값으로 시작되는 모든 요소
~~~
  <style>
    /* a 중에 href 값이 "https://"로 시작 */
    a[href^="https://"] { color: red; }
  </style>

  <body>
    <a href="https://www.google.com"> 선택!! </a><br>
    <a href="test.jsp"> 선택안됨 </a>
  </body>
~~~

10. 셀렉터[어트리뷰트$=”값”] : `a[href$=".html"] `  
    지정된 속성 값으로 끝나는 모든 요소
~~~
  <style>
    /* a 중에 href 값이 ".html"로 끝나는 요소 */
    a[href$=".html"] { color: red; }
  </style>

  <body>
    <a href="test.html"> 선택!! </a><br>
    <a href="test.jsp"> 선택안됨 </a>
  </body>
~~~    

11. 셀렉터[어트리뷰트 *= ”값”] : `div[class*="test"]`  
    지정된 속성값을 포함하는 요소
~~~
 <style>
    /* div 요소 중 class 값에 "test"를 포함하는 모든 요소 */
    div[class*="test"] { color: red; }

    /* div 요조 중 class 값에 공백으로 분리된 "test" 단어를 포함하는 요소 */
    div[class~="test"] { background-color: yellow; }
  </style>

  <body>
    <div class="first_test"> *= 'test' 적용됨 </div>
    <div class="second"> 적용대상 없음 </div>
    <div class="test"> *= 'test' 와 ~= 'test' 적용 </div>
    <p class="test"> 적용대상 없음 (div 요소가 아님) </p>
  </body>
~~~

***

## 복합 셀렉터  
![Alt text](./images/descendant-child.png) 

1. 후손 셀렉터 : 셀렉터A 셀렉터B  
   셀렉터 A의 모든 후손 요소중 셀렉터B와 일치하는 요소 선택
~~~ 
<style>
  div p { color: red; }
</style>

<body>
  <div>
    <p> 포함 </p>
    <span>
      <p> 포함</p>
    </span>
  </div>
  <p> 포함 안됨</p>
</body>
~~~

2. 자식 셀렉터 : 셀렉터A > 셀렉터B
~~~
 <style>
  div > p { color: red; }
 </style>

 <body>
 <div>
    <p> 포함 </p>
    <span>
      <p> 포함 안됨</p>
    </span>
  </div>
  <p> 포함 안됨</p>
</body>
~~~

3. 형제(동위) 셀렉터 : 셀렉터A + 셀렉터B  
![Alt text](./images/Sibling_Combinator.png)  

~~~
<style>
  /* p 요소 바로 뒤에 ul 선택 */
  p + ul { color: red; }
</style>

<body>
  <ul>
    <li> 포함 안됨 </li>
  </ul>

  <p> 셀렉터 P와 동등관계 ul </p>
  <ul>
    <li> 포함!</li>
  </ul>

  <h2> 셀렉터 P 없음 </h2>
  <ul>
    <li> 포함 안됨  </li>
  </ul>
</body>
~~~

4. 형제(동위) 셀렉터 : 셀렉터A ~ 셀렉터B 
~~~
 <style>
    /* p 뒤에 위치하는 ul 모두 선택 */
    p ~ ul { color: red; }
 </style>

 <body>
  <ul>
    <li> 여기 아님 </li>
  </ul>

  <p> 셀렉터 P 뒤에 나오는 모든 ul 찾아! </p>
  <ul>
    <li> 포함!!</li>
  </ul>

  <h2> 바로뒤 아니지만 OK </h2>
  <ul>
    <li> 포함!!  </li>
  </ul>
</body>
~~~  

***

## 가상 클래스 셀렉터 (Pseudo-Class Selector)  

클래스가 존재하지 않지만, 요소의 특정 상태에 따라 클래스를 임의로 지정하여 스타일을 정의해준다.  
가상 클래스는 (.) 마침표 대신에 (:)을 사용하며, CSS 표준에 의해 미리 정의된 이름이 있기 때문에 임의로 정의 할 수 없다.  

### 링크 셀렉터 (Link Pseudo-Class), 동적 셀렉터 (User action Pseudo-Class)  

* `:link` : 셀렉터가 방문하지 않은 링크  
* `:visited` : 셀렉터가 방문한 링크  
* `:hover` : 셀렉터에 마우스가 올라와 있을 떄  
* `:action` : 셀렉터가 클릭된 상태  
* `:focus` :  셀렉터에 포커스가 들어와 있을 떄  
* `:checked` : 셀렉터가 체크된 상태일 때  
* `:enabled` : 셀렉터가 사용가능한 상태일 때  
* `:disabled` : 셀렉터가 사용불가능한 상태일 떄
~~~
  <style>
     a:link { color: orange; }
     a:visited { color: green; }
     a:hover { font-weight: bold; } 
     a:active { color: blue; } 

     input[type=text]:focus, input[type=password]:focus {
      color: red;
    }

    input:enabled + span { color: blue;  }
    input:disabled + span { color: gray; } 
    input:checked + span {  color: red;  }
  </style>  
~~~  

### 구조 가상 클래스 셀렉터(Structural pseudo-classes)  

*  `:first-child` : 셀렉터에 해당하는 모든 요소 중 첫번째 자식 요소 선택  
* `:last-child` : 셀렉터에 해당하는 모든 요소 중 마지막 자식 요소 선택  
* `:nth-child(n)` : 셀렉터에 해당하는 모든 요소 중 앞에서 n번째 자식 요소 선택  
* `nth-last-child(n)` : 셀렉터에 해당하는 모든 요소 중 뒤에서 n번째 자식 요소 선택  
~~~
  <style>
   p:first-child { color: red; } 
   p:last-child { color: blue; }

   ol > li:nth-child(2)  { color: orange; } 
   ol > li:nth-last-child(2) { color: red; }
  </style>
 
  <body>
   <p> p요소중 첫번째 자식 : red 선택 </p> 
   <h1> 지정된 셀렉터 아님 </h1>
   <p> p요소지만 선택사항 없음 </p>
   <div>
    <p> div요소의 첫번째 p이기 때문에 : red 선택 </p>
    <p> 여기가 마지막 p 자식 : blue 선택 </p>
   </div>

   <ol>
    <li>Espresso</li>
    <li> nth-child(2) : orange 선택 됨 </li>
    <li>Caffe Latte</li>
    <li>Caffe Mocha</li>
    <li> :nth-last-child(2) : red 선택 됨</li>
    <li>Cappuccino</li>
   </ol>
  </style>
~~~  
> ol > li:nth-child(2n) 짝수번째 요소만 선택  
> ol > li:nth-child(2n+1) 홀수번째 요소만 선택 

### 부정 셀렉터(Negation pseudo-class)  
* `not(셀렉터)` : 셀렉터에 해당하지 않는 모든 요소 선택  
~~~
 <style>
    /* input 중에서 type 값이 password가 아닌 요소 선택 */
    input:not([type=password]) {
      background: yellow;
    }
  </style>  

  <body>
   <input type="text" value="Text input"> yellow 선택
   <input type="email" value="email input"> yellow 선택
   <input type="password" value="Password input"> 선택 안됨!!
  </body>
~~~

***


