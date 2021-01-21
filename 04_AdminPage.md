# 4강. 관리자 페이지(Admin page) 활용하기

## 1. admin.py를 통한 테이블 확인  
admin.py를 클릭하고 실행 버튼을 누른 후 해당 링크를 클릭하여 들어가면 Django 창이 뜸  
링크에서 '/amdin'을 추가하여 엔터를 누르면 로그인 창이 뜨는데  
### 이때 로그인 할 수 있는 계정(슈퍼유저)을 만들어 줘야 함  
아래 명령어를 입력하면 슈퍼유저를 만들 수 있음  
<pre><code>
# python manage.py createsuperuser
</code></pre>  
Username, Email address, Password(8자리)를 입력  
생성된 계정으로 로그인 창에 들어가 로그인을 하면 groom 초기 셋팅에서 서버 실행시 migrate 명령어로 생성된 장고의 기본 테이블들(User, Group)이 있음  
### CRUD(생성, 조회, 수정, 삭제) 모두 구성  

## 2. Post 모델을 관리자 페이지에서 관리  
admin.py 에 들어가서 아래의 명령어를 입력하여 모델을 가져옴  
<pre><code>
from django.contrib import admin  
  
admin.site.resgister()
</code></pre>  
하지만 현재 모듈(파일)에선는 Post 모델을 찾을 수 없기에 가져와야 함  
<pre><code>
from django.contrib import admin  
from .models import Post  
  
admin.site.resgister(Post)
</code></pre>  
### . 은 같은 경로 의미  
실행 후 새로고침을 하면 창에 Post가 나타남  

## 3. 커스터마이징  
