# 8강. Include로 HTML 소스 관리하기  

## 1. 예시
1. Navbar(메뉴바) 만들기(1)  
bootstrap 사이트에 들어가서 'Navbar'를 검색  
아래의 코드를 복사하여 base.html에서 body 밑에 바로 붙여넣기  
![navbar](https://user-images.githubusercontent.com/31130917/106144078-0bcfa880-61b7-11eb-8f64-db254d75191c.PNG)  
실행을 하면 아래와 같은 결과 창이 뜸  
![navbar결과](https://user-images.githubusercontent.com/31130917/106144317-5f41f680-61b7-11eb-96f4-a41c7202bdcb.PNG)  
이는 다른 링크 페이지에서도 똑같이 확인할 수 있음(base.html에 코드를 작성하였기 때문)  
  
2. Navbar(메뉴바) 만들기(2)  
위 1번의 경우 모든 곳에서 Navbar가 형성된다. 만약 특정 부분에만 생기게 하고싶다면 다음과 같이 함  
templates 폴더 > share 폴더 > _navbar.html 만듬  
이때 '_'을 붙인이유는 include를 사용한다는 것을 표시하고자 붙임(안 붙여도 됨)  
  
위 1번에서 base.html에 작성하였던 navbar 코드를 복사하여 _navbar.html에 붙여넣기함(base.html에 있는 것은 지움)  
base.html에서 Navbar코드를 작성하였던 곳에 아래의 코드를 작성  
<pre><code>
{% include 'share/_navbar.html' %}
</code></pre>  
실행 하면 1번과 같은 결과를 확인 할 수 있음  
  
#### main 페이지와 first 페이지만 Navbar가 필요한 경우  
base.html에 작성된 include 코드를 복사하여 main.html과 first.html의 'block content' 코드 밑에 붙여넣기 함(base.html에 있는 것은 지움)  
실행하면 main 페이지와 first 페이지만 Navbar가 생성되어 있는 것을 확인 할 수 있음  
  
### => include를 사용하면 코드 수정이 빠름(효율성이 높아짐)
