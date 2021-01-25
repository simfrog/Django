# 5강. HTML 띄우기

## 1. View에서 render를 통해 HTML을 보여주는 방법  
1. 경로를 만들어줌  
  ex) movie.naver.com -> 도메인 (구글에서는 진한색으로 표시)  
      '/english/django...' -> 경로  
  => url = 도메인 + 경로  
2. 그 경로에 해당하는 view를 할당  
  urls.py 의 urlpatterns에 해당 url 저장  
  urls.py -> Django에서 경로를 생성하고 어디로 갈지 알려주는 네비역할  
3. 해당 view에서 요청을 처리하여 응답  

## 2. 예시  
urls.py에서 경로 만듬  
<pre><code>
from django.contrib import admin  
from django.urls import path  

urlpatterns = [  
    path('admin/', admin.site.urls),  
    path('', 어디로가야하죠?)  
]
</code></pre>  
  
posts 앱(폴더) > views.py에 들어가서 함수를 작성  
views.py에서 코드를 짜는법 -> 함수로 View 짜기 or 클래스로 View 짜기  
<pre><code>
from django.shortcuts import render  
  
def main(request):  
    return render(request, 어떤 html을 띄어줄건지)
</code></pre>  
  
### posts 앱(폴더) > templates (폴더) > posts (폴더) > main.html 을 만듬  
이때 views.py의 함수 이름과 html 파일 이름을 같게 함  
render에서 html 등 파일을 가져올때 templates 이름의 폴더에서 가져옴  
만약 다른 앱(폴더)에 똑같은 이름의 다른 html이 있다면 다른 html을 가져올 수 있음  
-> templates폴더 안에 posts 앱(폴더)와 이름이 같은 폴더를 만들어줌  
<pre><code>
from django.shortcuts import render  
  
def main(request):  
    return render(request, 'posts/main.html')
</code></pre>  
  
urls.py에 main함수 넣어줌    
posts 앱(폴더) > views.py 에서 main함수 import 가져옴  
urls.py에 아래와 같이 작성  
<pre><code>
from django.contrib import admin  
from django.urls import path  
from posts.views import main  
  
urlpatterns = [  
    path('admin/', admin.site.urls),  
    path('',main),  
]
</code></pre>  
  
main.html은 아래와 같이 작성후 실행을 하면 다음과 같이 나옴  
![html파일](https://user-images.githubusercontent.com/31130917/105482677-0de3c400-5cec-11eb-8cca-0a1431c7dd6c.PNG)  
![html](https://user-images.githubusercontent.com/31130917/105482560-e42a9d00-5ceb-11eb-8c20-9d42f7332fbb.PNG)
