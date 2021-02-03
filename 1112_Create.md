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
실행하면 아래와 같은 결과를 확인 할 수 있음  
![메뉴바수정결과](https://user-images.githubusercontent.com/31130917/106691243-887ae080-6616-11eb-93ee-44dd55ecb017.PNG)  
  
6. posts/new 페이지에서 너비를 맞추고 게시글 작성란에 맡에 new.html에서 다음과 같이 코드 수정  
![html수정](https://user-images.githubusercontent.com/31130917/106691641-34243080-6617-11eb-8352-47a4659e2e3e.PNG)  
#### (mb-3안에 form-control 기억하기)  
실행하고 메뉴바에 '세글 쓰기'를 클릭하면 아래와 같은 결과를 확인 할 수 있음  
![게시글 결과](https://user-images.githubusercontent.com/31130917/106691781-751c4500-6617-11eb-80f4-402856b8263e.PNG)  
  
* ### Get 방식 : url로 접속을 해서 바로 그 페이지가 보이는 경우 대부분 Get 방식  
* ### Post 방식 : 어떤 처리를 해주는 방식  
  ### ex) 로그인 버튼을 누르면 바로 어떤 페이지로 떨어지는 것이 아닌 로그인 처리가 된 후 메인 페이지로 떨어짐  
  
7. form에 action 속성으로 form의 데이터를 어디로 보낼지 결정  
또한 입력한 값이 각각 어떤 이름으로 오는지 알려줘야함(input에서 name을 통해 정함)  
아래와 같이 코드를 작성  
![html수정2](https://user-images.githubusercontent.com/31130917/106692972-dee91e80-6618-11eb-8aef-21f145b22157.PNG)  
실행을 하고 글 작성 버튼을 클릭하면 위 url이 아래와 같이 변경된 것을 확인할 수 있음  
<pre><code>
도메인/경로/경로/?title=새로운&content=글입니다
</code></pre>  
#### 입력한 값과 해당 값의 이름이 주소에 보임 => Get 방식  
#### Get 방식을 사용할 경우 데이터 노출 위험 발생 따라서 Post 방식 사용  
  
데이터를 new가 아닌 'create'라는 곳을 새로 만들어 그곳에 보냄  
new.html을 아래와 같이 수정  
![html수정3](https://user-images.githubusercontent.com/31130917/106693740-6daa6b00-661a-11eb-8579-242c238745b1.PNG)  
  
posts 앱(폴더) > urls.py에도 추가 작성  
<pre><code>
from django.urls import path  
from .views import new, create  
  
app_name = "posts"  
urlpatterns = [  
    path('new/', new, name="new"),  
    path('create/', create, name="create"), // 추가 작성 부분  
]
</code></pre>  
  
posts 앱(폴더) > views.py에도 추가 작성  
<pre><code>
from django.shortcuts import render  
  
def main(request):  
    return render(request, 'posts/main.html')  
  
  
def new(request):  
    return render(request, 'posts/new.html')  
  
// 추가 작성 부분  
def create(request):  
    pass
</code></pre>  
이렇게 하면 데이터를 create로 보내줄 수 있음  
이제 데이터를 저장하는 방법에 대해 작성  
posts 앱(폴더) > views.py에서 다음과 같이 코드 수정  
<pre><code>
from django.shortcuts import render  
from .models import Post // ORM을 가져와서 Post객체 생성
  
  
def main(request):  
    return render(request, 'posts/main.html')  
  
  
def new(request):  
    return render(request, 'posts/new.html')  
  
  
def create(request):  
    if request.method == "POST": // 만약 request의 방식이 POST이면(POST방식만 처리하고자)  
        title = request.POST.get('title') // 지정해준 변수 = request.POST.get(POST방식으로 오는 데이터 이름)  
        content = request.POST.get('content')  
        Post(title=title, content=content).save() // Post 테이블에 다음과 같이 저장  
        //Post(Post 테이블의 column = 실제로 넘겨준 값, )
</code></pre>  
실행을 하고 글 작성을 하면 403 CSRF(Cross Site Request Forgery : 외부로부터 데이터를 보호해주는 장고의 토큰, 긴 문자열을 form에 데이터와 같이 넣어서 그 값이 고유하다는 것을 보여줘야 데이터를 정상적으로 처리해주는 보안 방식) 에러가 발생  
#### => new.html에 아래의 코드 추가 작성  
<pre><code>
form action=....
{% csrf_token %} // 추가 작성 부분  
div class=...
</code></pre>  
실행을 하고 글 작성을 하면 admin 페이지에서 생성이 된 것은 확인 할 수 있으나 에러창이 뜬 것을 확인 할 수 있음(어떤 Http 응답을 주지 않는다는 에러, 즉 예시로 네이버에서 로그인을 하면 다음 실행이 표시되어야 하는데 그대로인 경우)  
  
posts 앱(폴더) > views.py에서 다음과 같이 코드 추가및수정  
<pre><code>
from django.shortcuts import render, redirect  
from .models import Post  
  
  
def main(request):  
    return render(request, 'posts/main.html')  
  
  
def new(request):  
    return render(request, 'posts/new.html')  
  
  
def create(request):  
    if request.method == "POST":  
        title = request.POST.get('title')  
        content = request.POST.get('content')  
        Post(title=title, content=content).save()  
        return redirect('main') // main 페이지로 돌아간다는 뜻
</code></pre>  
실행을 하고 글 작성을 하면 메인 페이지로 돌아가고 admin 페이지에도 새로운 글이 생성된 것을 확인 할 수 있음  
Post 객체를 생성하는 다른 방법은 아래와 같이 수정해도 가능  
<pre><code>
Post(title=title, content=content).save() -> Post.objects.create(title=title, content=content) 으로 수정
</
