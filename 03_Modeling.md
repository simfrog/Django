# 3강. Django로 모델링  

## 1. 모델링을 통한 테이블 생성
1. models.py 에서 모델링  
2. python manage.py makemigrations로 주문서 만듬 (mirgration : 생성 될 테이블 정보를 담은 주문서)  
3. python manage.py migrate로 주문서 내역대로 테이블을 생성 (table : 모델의 설계를 그대로 데이터베이스에 저장한 형태)  

## 2. 파일 종류
* settings.py : Django의 설정을 담당하는 부분  
* addmin.py : 관리자 페이지 설정 관련 모듈  
* views.py : request를 처리해서 response를 내놓는 곳  

## 4. 데이터 타입  
* CharField : 문자열 데이터 타입 (max_length : 최대길이 설정)  
* TextField : 1000자 정도의 데이터 타입  
* IntegerField : 정수 타입 (default값 설정)  
* DateTimeField : 날짜시간 타입  
* auto_now_add : 생성될때 현재시간 저장  
* auto_now : 생성, 수정될때 현재시간 저장

## 5. Modeling 예시  
터미널에 아래와 같이 작성하면 app이 하나 만들어짐  
<pre><code>
# python manage.py startapp posts
</code></pre>  
  
만들어진 posts에서 models.py에 들어가 아래와 같이 작성  
<pre><code>
from django.db import models  
  
  
class Post(models.Model):  
    title = models.CharField(max_length=300)  
    content = models.TextField()  
    view_count = models.IntegerField(default=0)  
  
    created_at = models.DateTimeField(auto_now_add=True)  
    updated_at = models.DateTimeField(auto_now=True)
</code></pre>  
  
make migrations(주문서 만듬) -> migrate(주문서 적용)  
<pre><code>
# python manage.py makemigrations
</code></pre>
위 명령어를 입력하였을 때 이와 같이 나옴  
#### 'No changes detected'  
현재 Django에서는 posts 앱의 존재를 모른다는 뜻  
=> Django 폴더의 setting.py에 들어가서 INSTALLED_APPS 리스트 안에 'posts' 앱이름 추가  
다시 위 명령어를 입력하면 아래와 같은 결과 창이 뜸  
![모델링1](https://user-images.githubusercontent.com/31130917/105176990-5cb52080-5b69-11eb-869a-ac72b066e14e.PNG)  
  
Post폴터 안의 migrations에 주문서가 만들어짐  
* id : 기본키(primary key), 고유 식별 가능한 정보  
  한 테이블 안에서 동일한 id(primary key)를 갖는 객체는 없어야 함  
  
아래와 같은 명령어를 입력하면 만들어진 주문서대로 테이블이 만들어짐  
<pre><code>
# python manage.py migrate
</code></pre>
위 명령어를 입력하면 데이터베이스에 테이블 형태로 Post를 만든 것임  
  
ORM을 사용하여 실제로 테이블이 생성되었는지 확인해보고자 shell로 들어가야 됨. 이때 명령어는  
<pre><code>
# python manage.py shell
  
  
from posts.models import Post // 먼저 import를 함  
Posts.objects.create(title="간장게장 담그는 법", content="알이 꽉찬 꽃게를 간장에 담근다") // ORM을 통해 SQL과 소통  
=> <Post: Post object (1)> // 1이란 id값을 갖는 게시글의 객체  
Post.objects.all() // 모든 Post에 있는 게시글의 객체를 불러옴  
post = Post.objects.get(id=1) // post라는 변수에 id가 1인 객체를 넣음  
post.title // 제목 불러옴  
post.content // 내용 불러옴  
post.view_count // 조회수  
post.created_at // 생성날짜및시간 조회  
post.updated_at // 수정날짜및시간 조회
