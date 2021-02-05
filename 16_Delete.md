# 16강. Delete 삭제하기  

## 1. 예시(특정 게시글에서 '삭제하기' 버튼 만들기)  
1. posts 앱(폴더) > templates 폴더 > posts 폴더 > show.html에서 아래의 코드 추가  
![삭제하기버튼](https://user-images.githubusercontent.com/31130917/107024245-21625500-67eb-11eb-9044-c9acbe0a9682.PNG)  
실행하면 아래와 같은 결과를 확인 할 수 있음  
![삭제하기버튼결과](https://user-images.githubusercontent.com/31130917/107024248-21faeb80-67eb-11eb-999e-0fc96b658306.PNG)  
  
2. '삭제하기' 버튼 클릭시 '정말 삭제하시겠습니까?' 라는 Modal이 뜨게 하기  
bootstrap에 들어가서 'Modal'을 검색하고 'Live Demo'에서 아래의 코드를 복사하여 'class="btn btn-outline-danger"' 다음에 붙여넣기 함  
<pre><code>
data-bs-toggle="modal" data-bs-target="#exampleModal"
</code></pre>  
그리고 'Modal' 코드를 복사하여 아래와 같이 붙여넣기 함  
![modal](https://user-images.githubusercontent.com/31130917/107027419-7738fc00-67ef-11eb-9841-69d04717144e.PNG)  
실행하면 아래와 같은 결과를 확인 할 수 있음  
![modal결과](https://user-images.githubusercontent.com/31130917/107027427-786a2900-67ef-11eb-8a2d-7699c1ba05de.PNG)  
  
코드에서 'modal-body' 부분은 필요가 없기에 지움  
실행하면 아래와 같은 결과를 확인 할 수 있음  
![modal결과수정](https://user-images.githubusercontent.com/31130917/107027652-c97a1d00-67ef-11eb-94c6-99edb79c8db9.PNG)  
  
'modal-title' 란은 '정말 삭제하시겠습니까?'를  
'modal-footer' 밑에 있는 버튼 2개는 '닫기'와 '삭제'로 변경함  
2번째 버튼의 경우 class를 아래와 같이 변경  
<pre><code>
class="btn btn-outline-danger"
</code></pre>  
실행하면 아래와 같은 결과를 확인 할 수 있음  
![modal결과수정2](https://user-images.githubusercontent.com/31130917/107028110-7359a980-67f0-11eb-9076-f581bfb978e8.PNG)  
  
3. '삭제하기' 벝튼 클릭시 진짜 삭제되기  
posts 앱(폴더) > urls.py에서 아래와 같이 코드 추가  
<pre><code>
from django.urls import path  
from .views import new, create, show, edit, update, delete  

app_name = "posts"  
urlpatterns = [  
    path('new/', new, name="new"),  
    path('create/', create, name="create"),  
    path('<int:post_id>/', show, name="show"),  
    path('edit/<int:post_id>/', edit, name="edit"),  
    path('update/<int:post_id>/', update, name="update"),  
    path('delete/<int:post_id>/', delete, name="delete"), // 추가 작성 부분  
]
</code></pre>  
  
#### deletes는 POST 방식이 적합  
posts 앱(폴더) > views.py에서 맨 밑에 아래의 코드 추가  
<pre><code>
...  
def delete(request, post_id):  
    if request.method == "POST":  
        post = Post.objects.get(pk=post_id)  
        post.delete()  
        return redirect('main')
</code></pre>  
  
#### a link 로는 GET 방식의 접근밖에 안되서 form 사용  
posts 앱(폴더) > templates 폴더 > posts 폴더 > show.html에서 '삭제'버튼의 코드를 삭제하고 아래의 코드로 바꿔줌  
![삭제버튼수정](https://user-images.githubusercontent.com/31130917/107029222-08a96d80-67f2-11eb-8b9b-9e75e1d72620.PNG)  
실행하여 실제로 '삭제' 버튼을 클릭하면 해당 게시글이 삭제된 것을 확인 할 수 있음(생략)
