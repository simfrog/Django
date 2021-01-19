# Django

## 1. 개념
* ### 프레임워크 : 뼈대나 기반구조를 의미. 소프트웨어의 특정 문제를 해결하기 위해 상호 협력하는 클래스와 인터페이스의 집합  

## 2. 웹 프레임워크에서 역할
* UI : HTML, CSS 등  
* 데이터 베이스 : Oracle, MySQL  
* URL 파싱  
* 세션 관리  
* 관리자 페이지  
* Request 파싱 등등  

## 3. 3단 구조
### 모델(Model), 템플릿(Template), 뷰(View)  
### => MTV 패턴  
![mtv](https://user-images.githubusercontent.com/31130917/105035589-95d48e80-5a9e-11eb-913e-e1412a18c573.PNG)  
* ### View : 요청에 대한 응답을 하는 곳  
  ex) url 접속 -> 11번가 서버로 request -> 11번가 홈페이지 보임 -> 11번가 서버의 response  
* ### Template : View에서 response로 쓰이는 HTML 등등 (render를 통해 template을 responce로 client에게 보여줌)  
  ex) View => 11번가의 서버, Template => 11번가 화면  
* ### Model : Modeling을 통해 만들어짐. 추상적인 개념. DataBase에 테이블 형태로 만들기 위한 설계  
* ### DataBase : 실제 데이터를 저장하는 곳 (SQL 씀. ORM을 통해 파이썬과 데이터베이스 사이에서 통역사 역할)  
* ### column, field, attribute : 특정 모델의 속성들 (데이터 타입 지정 필요)  
(migration을 통해 database에 table 생성)  
* ### CreatedAt(생성시간), UpdatedAt(수정시간) : 객체가 언제 생성되고 수정되었는지 중요  
* ### Relation : Model 상호간의 관계  

## 4. 장고(Django)가 제공하는 것  
* 폼  
* 개발 프로세스  
* 관리자  
* 보안 등등
