# 17강. CURD 코드 리펙토링  

## 1. reverse 함수  
### reverse 함수를 통해 특정 경로를 추적  
### urls.py에 선언해둔 name에 따라 url을 받아와서 사용할 수 있게 해주는 기능  
#### string 타입으로 출력  
  
1. 터미널에 아래의 코드를 입력하여 shell을 열음  
<pre><code>
# python manage.py shell
</code></pre>  
  
2. 아래의 코드를 입력하면 특정 경로가 string 타입으로 출력  
<pre><code>
from djngo.urls import reverse  
reverse('posts:new')  //reverse('특정 경로')  
=> '/posts/new/' (출력 결과)
</code></pre>  
  
3. 아래의 코드를 추가 입력하면 id 값을 출력  
1) args(arguments)  
<pre><code>
reverse('posts:show', args=[1]) // show에서 id 값이 1인 객체 출력  
// reverse('특정 경로'. args[id값])
</code></pre>  
2) kwargs(keyword arguments)  
<pre><code>
reverse('posts:show', kwargs={'post_id': 10}) // 그냥 'id'가 아닌 어떤 id인지 이름까지 적어줘야 함  
=> '/posts/10/' (출력 결과)
</code></pre>  
  
### redirect 함수는 resolve_url 이라는 함수를 사용  
1. 아래의 코드를 입력하면 특정 경로를 만듬  
<pre><code>
from djngo.shortcuts import resolve_url  
resolve_url('posts:show': 10)  
=> '/posts/10/' (출력 결과)
</code></pre>  
### => resolve_url은 내부적으로 reverse 사용  
  
### reverse 함수를 이용하여 현재 사용하고 있는 model에 'get_absolute_url'이라는 메소드를 오버라이드 해줄 수 있음  
1. posts 앱(폴더) > model.py에서 아래의 코드 추가  
<pre><code>
...  
from django.urls import reverse  
...  
  def get_absolute_url(self):  
          return reverse('posts:show', args=[self.pk])
</code></pre>  
=> reverse 함수를 이용해 객체의 id값을 이용해 주소를 만들어서 반환함  
### => 상세 페이지 만들 때 사용  
#### urls.py에서 url이 변경되더라도 url reverse를 이용해 변경된 url을 추적 할 수 있음  
  
## 2. 예시('get_absolute_url'을 이용하여 url을 조금 더 유연하게 작성하기)  
1. posts 앱(폴더) > templates 폴더 > posts 폴더 > main.html에서 href 부분을 아래의 코드로 수정  
<pre><code>
"{{ post.get_absolute_url }}"
</code></pre>  
실행하면 위 코드는 model.py의 get_absolute_url함수에서 상세 페이지 경로를 반환해주기 때문에  
main.html에서 template 변수로 입력하여주면 경로가 그대로 나오게 됨  
#### => urls.py 에서 경로가 변경되도 아무런 상관 없음  
실행하면 정상적으로 작동되는 것을 확인할 수 있음(생략)  
  
2. posts 앱(폴더) > views.py에서 'update'함수의 redirect 부분을 아래와 같이 바꿔줘도 정상 작동  
<pre><code>
return redirect(post)
</code></pre>  
  
3. posts 앱(폴더) > views.py에서 'create'함수의 redirect 부분을 아래와 같이 바꿔줘도 정상 작동(상세 페이지로 잘 이동)  
<pre><code>
return redirect(form.instance)
</code></pre>  
  
## 3. 예시('get_object_or_404'를 이용하여 없는 페이지에 오류가 아닌 페이지가 없다는 창 뜨게 하기)  
1. shell에서 아래와 같이 코드 입력  
<pre><code>
from posts.models import Post  
post = Post.objects.get(pk=2)  
post  
</code></pre>  
아래와 같이 출력 됨  
![shell](https://user-images.githubusercontent.com/31130917/107114592-4c5fae00-68aa-11eb-8329-f89264d7a0f3.PNG)  
  
2. posts 앱(폴더) > models.py 에서 아래의 코드 추가  
<pre><code>
...  
def __str__(self):  
        return self.title  
  
def ...
</code></pre>  
exit으로 shell을 나가고 실행한 다음 다시 shell을 열어서 아래의 코드 입력  
<pre><code>
from posts.models import Post  
post = Post.objects.get(pk=7)  
post  
</code></pre>  
아래와 같이 출력 됨  
![shell수정](https://user-images.githubusercontent.com/31130917/107114705-59c96800-68ab-11eb-8323-6f76a37b25b2.PNG)  
  
3. posts 앱(폴더) > models.py에서 맨 위에 아래의 코드 추가  
<pre><code>
from django.shortcuts import render, redirect,  get_object_or_404
</code></pre>  
  
shell에서 아래와 같이 코드 입력하면 1번과 같이 출력되는 것을 확인 할 수 있음  
<pre><code>
from django.shortcuts import get_object_or_404  
post = get_object_or_404(Post, pk=7)  
post
</code></pre>  
없는 pk를 입력하면 1번과 다르게 에러가 아닌 아래와 같이 출력 됨  
![http404](https://user-images.githubusercontent.com/31130917/107115896-065b1800-68b3-11eb-901e-08082cd7e42e.PNG)  
  
4. posts 앱(폴더) > views.py에서 각 함수의 아래와 같은 코드를 변경함  
<pre><code>
post = Post.objects.get(pk=post_id) -> post = get_object_or_404(Post, pk=post_id)
</code></pre>  
실행한 후 잘못된 경로를 입력하면 아래와 같이 뜨는 것을 확인 할 수 있음  
![404](https://user-images.githubusercontent.com/31130917/107115965-99944d80-68b3-11eb-9e39-69b1975a66f6.PNG)  
  
## 3. 예시(데코레이터 를 이용한 코드 간결)  
* ### 데코레이터 : 함수나 클래스를 꾸며주는 역할  
1. posts 앱(폴더) > views.py에서 아래의 코드 추가  
<pre><code>
...  
from django.views.decorators.http import require_POST  
...
</code></pre>  
=> POST 방식만 허용하는 view가 됨  
  
야래와 같이 코드를 추가하면 'if request.method == "POST":'이러한 기존의 코드가 없어져도 됨  
<pre><code>
...  
@require_POST  
def create(request):  
      form = PostForm(request.POST)  
      if form.is_valid():  
          form.save()  
      return redirect(form.instance)  
...
</code></pre>  
'update', 'delete'함수도 위와 같이 수정함
