# 6강. 링크 타고 다른 페이지 넘어가기  

## 1. 예시1  
1. 새로운 앱(폴더)를 만듬  
<pre><code>
# python manage.py startadpp lovely
</code></pre>  
후에 settings.py의 INSTALLED_APPS에 추가(뒤에 ,표시 꼭 입력)  
  
urls.py에 들어가 아래와 같이 코드 추가  
<pre><code>
from django.contrib import admin  
from django.urls import path  
from posts.views import main  
  
urlpatterns = [  
    path('admin/', admin.site.urls),  
    path('',main),  
    path('first/',...),  
    path('second/',...),  
    path('third/',...),  
]
</code></pre>  
  
2. 어떤 html을 render 해줄지  
새로 만든 앱(폴더)의 views.py에 들어가 아래와 같이 작성  
<pre><code>
from django.shortcuts import render  
  
def first(request):  
    return render(request, '...')  
  
  
def second(request):  
    return render(request, '...')  
  
  
def third(request):  
    return render(request, '...')
</code></pre>  
함수 간격을 2줄씩 띄우는 것이 보기 좋음  
  
3. 만든 앱(폴더) > templates 폴더 > 만든 앱(폴더)과 같은 이름의 폴더 > html 만듬  
해당 예시의 경우 lovely 앱(폴더) > templates 폴더 > lovely 폴더 > first,second,third.html 만듬  
그리고 3개의 각 html 파일에 5강에서와 같은 코드 작성(사진 생략)  
  
views.py에 들어가 아래와 같이 코드 수정  
<pre><code>
from django.shortcuts import render  
  
  
def first(request):  
    return render(request, 'lovely/first.html')  
  
  
def second(request):  
    return render(request, 'lovely/second.html')  
  
  
def third(request):  
    return render(request, 'lovely/third.html')
</code></pre>  
  
그리고 urls.py로 들어가 아래와 같이 코드 수정  
<pre><code>
from django.contrib import admin  
from django.urls import path  
from posts.views import main  
from lovely.views import first, second, third  
  
urlpatterns = [  
    path('admin/', admin.site.urls),  
    path('',main),  
    path('first/', first),  
    path('second/', second),  
    path('third/', third),  
]
</code></pre>  
작성 후 실행을 하면 아래와 같은 결과가 나타남  
![링크1](https://user-images.githubusercontent.com/31130917/105709754-7edbe380-5f59-11eb-9714-157ec4a3415d.PNG)  
![링크2](https://user-images.githubusercontent.com/31130917/105709761-800d1080-5f59-11eb-8e16-d378135dc19b.PNG)  
![링크3](https://user-images.githubusercontent.com/31130917/105709763-800d1080-5f59-11eb-8512-fcf7eff08199.PNG)  
  
## 2. 예시2  
예시1은 모든 경로를 urls.py에서 관리함  
경로가 너무 많아지면 관리하기 힘들어짐  
### => 'include'를 이용하여 관리  
  
urls.py에 아래와 같이 수정  
<pre><code>
from django.contrib import admin  
from django.urls import path, include  
from posts.views import main  
from lovely.views import first, second, third  
  
urlpatterns = [  
    path('admin/', admin.site.urls),  
    path('',main),  
    path('lovely/', include('lovely.urls')), // lovely 관련 경로는 lovely.urls 파일에 맡기겠다는 뜻  
]
</code></pre>  
  
lovely 앱(폴더)에 urls.py 생성하고 아래와 같이 코드 작성  
<pre><code>
from django.urls import path  
from .views import first, second, third  
  
urlpatterns = [  
    path('first/',first),  
    path('second/', second),  
    path('third/', third),  
]
</code></pre>  
작성 후 실행을 하고 '/first'하면 뜨지 않음  
'/lovely/first'로 입력하면 예시1과 같은 결과창이 뜸  
### => include 쓰는 것을 권장  
  
### 3. 메인 화면에 링크 달기  
1. main.html 파일에 들어가 아래와 같이 코드 추가  
5강에서 post 앱(폴더) 안에 만들어져 있음  
![html](https://user-images.githubusercontent.com/31130917/105712476-30c8df00-5f5d-11eb-95b4-b2fd4be561b3.PNG)  
다음과 같은 결과창이 뜸  
![html결과](https://user-images.githubusercontent.com/31130917/105712479-31617580-5f5d-11eb-9bd4-2da1f07788b1.PNG)  
  
해당 링크를 클릭하였을 때 다른 페이지로 넘어갈 수 있도록 아래와 같이 코드 수정  
![html_path](https://user-images.githubusercontent.com/31130917/105712673-79809800-5f5d-11eb-95fe-fe0665ebf42f.PNG)  
위와 같이 작성할 경우 경로가 길어지면 코드가 길어짐 따라서 아래와 같이 수정  
  
urls.py  
<pre><code>
from django.contrib import admin  
from django.urls import path, include  
from posts.views import main 
from lovely.views import first, second, third  
  
urlpatterns = [  
    path('admin/', admin.site.urls),  
    path('',main, name="main"),  
    path('lovely/', include('lovely.urls')),  
]
</code></pre>  
  
lovely 앱(폴더)의 urls.py  
<pre><code>
from django.urls import path  
from .views import first, second, third  
  
urlpatterns = [  
    path('first/',first, name="first"), // path(경로부분, import한 함수 이름, 경로를 불러주는 이름)  
    path('second/', second, name="second"),  
    path('third/', third, name="third"),  
]
</code></pre>  
  
main.html에 가서 아래와 같이 코드 작성  
* template 언어 : Django의 html에서 쓰이며 파이썬과 비슷함  
![html_template](https://user-images.githubusercontent.com/31130917/105713719-cd3fb100-5f5e-11eb-8a31-41d79e2a609e.PNG)  
같은 결과가 나오는 것을 확인 할 수 있음(생략)  
  
만약 다른앱에 똑같은 이름의 url이 필요한 경우 경로이름 앞에 app의 이름을 붙여줌  
아래와 같이 코드 수정  
  
lovely 앱(폴더)의 urls.py  
<pre><code>
from django.urls import path  
from .views import first, second, third  
  
app_name = "lovely"  
urlpatterns = [  
    path('first/',first, name="first"), // path(경로부분, import한 함수 이름, 경로를 불러주는 이름)  
    path('second/', second, name="second"),  
    path('third/', third, name="third"),  
]
</code></pre>  
  
main.html  
![html_최종](https://user-images.githubusercontent.com/31130917/105714228-6cfd3f00-5f5f-11eb-87a1-755460e12da8.PNG)  
같은 결과가 나오는 것을 확인 할 수 있음(생략)
