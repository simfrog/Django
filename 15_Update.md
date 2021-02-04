# 15강. Update 수정하기  

## 1. 예시('수정하기'를 클릭하였을 시 기존의 새로운 글 작성하기 페이지가 뜸, 이때 기존 게시글 객체의 데이터가 그대로 폼에 띄어주어야 함)  
1. posts 앱(폴더) > templates 폴더 > posts 폴더 > show.html에서 아래와 같이 코드 추가  
![수정하기버튼](https://user-images.githubusercontent.com/31130917/106897674-9aec3b80-6736-11eb-81c8-890fb8a0caf9.PNG)  
실행하면 아래와 같은 결과를 확인 할 수 있음  
![수정하기버튼결과](https://user-images.githubusercontent.com/31130917/106897765-b6574680-6736-11eb-865b-876aa42a8437.PNG)  
  
2. posts 앱(폴더) > urls.py에 아래의 코드를 추가  
<pre><code>
...  
from .views import new, create, show, edit  
  
urlpatterns = [  
...  
path('edit/<int:post_id>/', edit, name="edit"),  
]
</code></pre>  
특정 게시글 객체를 수정하는 것이기 때문에 id 필요  
  
2. posts 앱(폴더) > views.py 맨 밑에 아래의 코드를 추가  
<pre><code>
def edit(request, post_id):  
    post = Post.objects.get(pk=post_id)  
    context = {  
        'form': PostForm(instance=post)  
    }  
    return render(request, 'posts/edit.html', context)  
</code></pre>  
#### => instance로 post를 설정함으로써 post 각 객체의 title, content를 불러옴  
  
3. posts 앱(폴더) > templates 폴더 > posts 폴더 > edit.html 만듬  
아래와 같이 코드 작성  
![edit](https://user-images.githubusercontent.com/31130917/106899489-c3753500-6738-11eb-80f0-70fc3065e5f4.PNG)  
#### POST 방식에는 csrf_token이 필요  
  
4. posts 앱(폴더) > templates 폴더 > posts 폴더 > show.html에서 'href' 부분을 다음과 같이 수정  
<pre><code>
"#" -> "{% url 'posts:edit' post.pk %}"
</code></pre>  
실행하여 '수정하기' 버튼을 클릭하면 다음과 같은 결과를 확인 할 수 있음  
![수정하기결과](https://user-images.githubusercontent.com/31130917/106899911-45655e00-6739-11eb-8ee3-1cfbef057d9a.PNG)  
  
5. '수정하기' 버튼 클릭시 수정되기  
posts 앱(폴더) > urls.py에서 아래의 코드 추가  
<pre><code>
...  
from .views import new, create, show, edit, update  
  
...  
  path('update/<int:post_id>/', update, name="update"),  
]
</code></pre>  
  
posts 앱(폴더) > views.py에서 아래의 코드 추가  
<pre><code>
...  
def create(request):  
    if request.method == "POST":  
        form = PostForm(request.POST)  
        if form.is_valid():  
            form.save()  
        return redirect('main')  
...  
// 추가 작성 부분  
def update(request, post_id):  
    if request.method == "POST":  
        post = Post.objects.get(pk=post_id)  
        form = PostForm(request.POST, instance=post)  
        if form.is_valid():  
            form.save()  
        return redirect('posts:show', post_id)
</code></pre>  
#### => 위 create 함수에서는 'form = PostForm(request.POST)'와 같이 명령어를 씀으로써 다른 페이지가 생성될 수 있도록 하였으나  
   #### update 함수에서는 'form = PostForm(request.POST, instance=post)'와 같이 instance를 추가함으로써 기존 instance에 update될 수  
   #### 있게끔 함  
  
posts 앱(폴더) > templates 폴더 > posts 폴더 > edit.html에서 'form action='에 아래의 코드 추가  
<pre><code>
"{% url 'posts:update' post.pk %}"
</code></pre>  
이때 posts 앱(폴더) > views.py에 들어가서 아래와 같이 코드를 추가함으로써 edit.html에서도 post를 사용할 수 있음  
<pre><code>
...  
def edit(request, post_id):  
    post = Post.objects.get(pk=post_id)  
    context = {  
        'post': post,  
        'form': PostForm(instance=post)  
    }  
    return render(request, 'posts/edit.html', context)  
...
</code></pre>  
### 잊지 말기!  
실행하여 글을 수정하여보면 수정된 것을 확인할 수 있음(생략)  
  
## 2. 예시(비슷한 두 html 파일의 코드 정리 => include 이용)  
1. posts 앱(폴더) > templates 폴더 > posts 폴더 > _form.html 만듬  
  
2. 현재 html에서 사용가능한 데이터를 _form.html로 넘겨줌  
아래의 코드를 _form.html에 붙여넣기 함  
![include](https://user-images.githubusercontent.com/31130917/106914644-903aa200-6748-11eb-9e69-f1e87256231a.PNG)  
#### 이때 바뀔 가능성이 있는 부분은 include html 변수로 만들어줌  
![_form](https://user-images.githubusercontent.com/31130917/106915100-0212eb80-6749-11eb-8a5f-bb6d72062615.PNG)  
  
3. 복사한 edit.html로 가서 복사한 부분을 아래와 같이 코드 수정  
<pre><code>
{% include 'posts/_form.html' with form=form btn_value="글수정" %}
</code></pre>  
### 'with ...' 부분은 어떤 변수를 들고 갈건지에 대한 코드
실행하면 예시 1번과 동일한 결과를 확인 할 수 있음  
  
new.html로 가서 위와 같이 수정  
<pre><code>
{% include 'posts/_form.html' with form=form btn_value="글생성" %}
</code></pre>  
실행하면 예시 1번과 동일한 결과를 확인 할 수 있음
