# 11강. Create 생성하기  

## 1. 예시(게시글 작성 페이지 만들기)  
1. posts 앱(폴더)에서 url을 관리하고자 프로젝트 폴더의 urls.py에 아래의 코드를 추가 작성  
<pre><code>
from django.contrib import admin  
from django.urls import path, include  
from posts.views import main  
from lovely.views import first, second, third  
  
urlpatterns = [  
    path('admin/', admin.site.urls),  
    path('',main, name="main"),  
    path('lovely/', include('lovely.urls')),  
    path('posts/', include('posts.urls')), // 추가 작성 부분  
]
</code></pre>  
  
2. posts 앱(폴더) 안에 urls.py를 만들고 다음과 같이 코드 작성  
<pre><code>
from django.urls import path  
from .views import new  
  
app_name = "posts"  
urlpatterns = [  
    path('new/', new, name="new"),  
]
</code></pre>  
  
3.posts 앱(폴더) 안에 view.py에 아래와 같이 코드 추가 작성  
<pre><code>
from django.shortcuts import render  
  
def main(request):  
    return render(request, 'posts/main.html')  
  
// 추가 작성 부분  
def new(request):  
    return render(request, 'posts/new.html')
</code></pre>  
  
4. posts 앱(폴더) > templates 폴더 > posts 폴더 안에 new.html 만듬  
bootstrap에서 form을 검색하여 'Overview'부분을 복사한다음 아래 코드의 빈 공간에 붙여넣기  
<pre><code>
{% extends 'base.html' %}  
  
{% block content %}  
// 해당 부분에 붙여넣기  
{% endblock %}
</code></pre>  
  
main.html에 있는 navbar를 base.html로 옮김  
block content 위에 붙여넣기  
  
5. 프로젝트 폴더 > templates 폴더 > share 폴더 > _navbar.html에 다음과 같이 코드 수정  
![메뉴바수정](https://user-images.githubusercontent.com/31130917/106691178-66815e00-6616-11eb-887d-d81b5fc2171e.PNG)  
아래와 같은 결과를 확인 할 수 있음  
![메뉴바수정결과](https://user-images.githubusercontent.com/31130917/106691243-887ae080-6616-11eb-93ee-44dd55ecb017.PNG)  
  
6. posts/new 페이지에서 너비를 맞추고자 new.html에서 아래의 코드를 추가 작성  
'<div class="container"></div>' 사이에 bootstrap에서 가져온 코드를 붙여넣음  
