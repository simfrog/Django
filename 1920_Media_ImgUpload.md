# 19강. MEDIA와 이미지 업로드  

## 1. 개념  
* ### Media : 사용자가 업로드한 img, video, pdf 같은 정적파일  
  
## 2. 예시(MEDIA setting)  
1. image 정보를 저장 할 수 있는 필드 추가하기  
posts 앱(폴더) > models.py 에서 아래의 코드 추가  
<pre><code>
_type ...  
image = models.ImageField()  
...  
</code></pre>  
#### 이때 Pillow package 설치해야 함  
* ### Pillow : ImageField, FileField를 사용하기 위한 package  
  
#### 보통 local에서 Django 개발을 할 때는 가상환경을 사용  
  
2. 가상환경 설치  
터미널에서 아래의 코드를 입력하여 설치  
<pre><code>
# pip3 install virtualenv
</code></pre>  
  
가상환경 이름 설정
<pre><code>
# virtualenv 가상환경이름  
// 여기서는 가상환경이름을 myenv라 함
</code></pre>  
  
'ls'라고 입력하면 myenv가 생겨난 것을 확인 할 수 있음(생략)  
현재는 가상환경을 활성화 시키지 않았기 때문에 가상환경 밖에 있는 것임  
  
아래 코드를 입력하여 설치되어있는 pip package들을 살펴 볼 수 있음  
<pre><code>
# pip list
</code></pre>  
살펴보면 'Pillow'도 설치되어있는 것을 확인 할 수 있음(생략)  
  
3. 가상환경 활성화  
아래의 코드를 입력하여 가상환경을 활성화 함  
<pre><code>
# source myenv/bin/activate
</code></pre>  
왼쪽에 '(가상환경이름)'이라고 나타나며 가상환경의 활성화 된 것을 확인 할 수 있음  
  
아래 코드를 입력하여 설치되어있는 pip package들을 살펴 볼 수 있음  
<pre><code>
# pip list
</code></pre>  
가상환경 밖의 package와는 다르게 내부에만 있는 package를 사용하는 것을 확인 할 수 있음(생략)  
  
4. 가상환경 안에서 Pillow 설치  
아래 코드를 입력하여 Pillow 설치  
<pre><code>
# pip install pillow
</code></pre>  
'pip list' 명령어를 통해 설치 되었음을 확인 할 수 있음(생략)  
  
5. 어디에 업로드할지 지정하기  
프로젝트 폴더 > settings.py의 맨 밑에 아래의 코드 추가  
<pre><code>
MEDIA_URL = '/media/' // media 경로 설정  
MEDIA_ROOT = os.path.join(BASE_DIR, 'media') // media 루트 설정(BASE_DIR는 프로젝트 파일이 있는 경로를 뜻함)
</code></pre>  
  
posts 앱(폴더) > models.py 에서 아래의 코드 추가  
<pre><code>
_type ...  
image = models.ImageField(upload_to="posts/img")  
...  
</code></pre>  
#### => 앱들이 있는 프로젝트 폴더 > media 폴더 > posts 폴더 > img 폴더 > 업로드한 img  
#### 하지만 실제 DB 필드에서는 '/media/posts/img'경로에 저장  
  
6. 주문서 만들어 변경사항 반영  
=> 장고로 가져올 수 없다는 에러 발생  
'pip list'로 확인했을 때 Django가 깔려있지 않은 것을 확인 할 수 있음  
  
가상환경을 나가서 주문서 만듬  
<pre><code>
# deactivate
</code></pre>  
  
저번 강의와 같은 상황 발생  
=> 2번 선택  
posts 앱(폴더) > models.py에서 아래와 같이 코드 추가  
<pre><code>
image = models.ImageField(upload_to="posts/img", blank=True)  
// blank 대신 기본 이미지를 설정해도 됨  
// blank=True : 특정 컬럼값이 None 값임을 허용함
</code></pre>  
다시 주문서 만드는 코드 입력  
정상적으로 migration 파일이 생성된 것을 확인 할 수 있음(생략)  
migrate 명령어를 통해 실제 DB에 변경사항 반영  
  
프로젝트 폴더 > settings.py로 가면 기본 환경이 아래와 같이 된 것을 확인 할 수 있음  
<pre><code>
DEBUG = True
</code></pre>  
#### => 개발환경(Development)에 있다는 뜻(실제로 어떤 서버에 배포한 프로그램 상태가 아니라는 뜻)  
### static 파일은 개발환경에 기본으로 image를 서빙 할 수 있는 기능을 제공하지만 medial의 경우 그렇지 않음  
  
7. static 함수를 이용하여 서빙을 지원해주도록 함  
프로젝트 폴더 > urls.py에 아래의 코드를 추가  
<pre><code>  
...  
from django.conf import settings // settings를 가져옴  
from django.conf.urls.static import static // static 함수를 가져옴
</code></pre>  
  
아래와 같이 코드 추가  
<pre><code>
urlpatterns = [  
    path('admin/', admin.site.urls),  
    path('',main, name="main"),  
    path('lovely/', include('lovely.urls')),  
    path('posts/', include('posts.urls')),  
] + static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT) // 추가 작성 부분
</code></pre>  
#### static 함수의 경우 리스트를 리턴함(urlpatterns는 리스트 형태)  
실제로 프로덕션에서는 빈리스트를 리턴하기 때문에 크게 의미는 없으나 개발환경에서는 필요  
  
실행하여 관리자 페이지 > post 에서 아무 게시글에 들어가 이미지 추가를 하여 저장을 하면  
프로젝트에 media 폴더가 생성된 것을 확인 할 수 있고  
설정한 경로대로 media > posts > img에 업로드한 이미지가 저장되어 있는 것을 확인 할 수 있음(생략)  
  
# 20강. MEDIA와 이미지 업로드  

1. 예시(이미지 업로드)  
1. 현 상태에서 바로 실행을 하여 새글쓰기를 하면 에러발생  
#### => _type에 default값을 넣어주지 않았기 때문  
  
2. 이미지를 업로드하기위한 image 필드 추가  
posts 앱(폴더) > forms.py에서 'fields' 리스트에 'image' 추가  
'labels' 리스트에는 아래의 코드 추가  
<pre><code>
'image': '게시글 사진'
</code></pre>  
실행하면 아래와 같은 결과를 확인 할 수 있음  
![이미지업로드버튼](https://user-images.githubusercontent.com/31130917/107253909-27ba3080-6a7a-11eb-9700-84f5639ae988.PNG)  
  
3. 게시글의 종류(_type) 선택할 수 있도록 만들기  
* ### ChoiceField(선택 할 수 있는 필드) : widget이 아니므로 forms field로 만들어줘야 함  
posts 앱(폴더) > forms.py에서 아래의 코드 추가  
<pre><code>
class PostForm(forms.ModelForm):  
    _type = forms.ChoiceField( // 추가 작성 부분  
        widget=forms.RadioSelect,  
        label="게시글 종류",  
        choices=Post.POST_TYPES,  
        required=True  
    )  
    class Meta:  
        model = Post  
        fields = ['title', '_type', 'content', 'image'] // 추가 작성 부분  
...
</code></pre>  
* #### Radio : 다수 중 하나만 선택  
* #### CheckBox : 다수 중 여러개 선택  
실행하면 아래와 같은 결과를 확인 할 수 있음  
![라디오버튼](https://user-images.githubusercontent.com/31130917/107255404-9a77db80-6a7b-11eb-8292-2a755aa850ba.PNG)  
실제로 작성을 하여보면 작성된 것을 확인 할 수 있음 또한 관리자 페이지에서도 작성된 것을 확인 할 수 있음(생략)  
하지만 사진이 저장되어 있지 않은 것을 확인 할 수 있음(생략)  
  
4. 사진 업로드시 저장되기  
posts 앱(폴더) > admin.py에서 'list_display'에서 'id'밑에 'image' 추가  
실행하면 관리자 페이지 컬럼에 필드가 추가된 것을 확인 할 수 있음(생략)  
하지만 방금 생성한 게시글에는 이미지가 정상적으로 업로드 되지 않은 것을 확인 할 수 있음(생략)  
  
### 원인  
#### 1) 인코딩 문제  
posts 앱(폴더) > views.py에서 post 변수 밑에 'pdb.set_trace()' 입력하여 디버깅  
아래의 코드 입력  
<pre><code>
request.POST  
</code></pre>  
#### 잘 된 것처럼 보이지만 ImageField에는 string type을 저장 할 수 없고, string은 이미지 형식이 아님  
#### => POST가 딕셔너리, 즉 string type으로 와서 저장이 안됨  
  
posts 앱(폴더) > templates 폴더 > posts 폴더 > edit.html에서 method 뒤에 아래의 코드 추가  
<pre><code>
enctype="multipart/form-data">
</code></pre>  
#### => form에 인코딩 옵션을 추가해서 파일을 정상적으로 데이터 전송 할 수 있게 함  
  
new.html도 edit.html과 똑같이 해줌  
다시 디버깅하여 'request.POST'를 입력하면 이번에는 이미지가 아예 없는 것을 확인 할 수 있음(생략)  
#### => request.POST에 담겨져서 오는 것이 아닌 request.FILES로 옴  
#### 'request.FILES'를 입력하면 image라는 key 값에 image 정보를 담은 객체 형태로 옴  
  
posts 앱(폴더) > views.py에서 update 함수의 form 변수에 아래와 같이 코드 수정  
<pre><code>
form = PostForm(request.POST, request.FILES, instance=post)
</code></pre>  
create 함수에도 적용  
이미지를 항상 업로드하는 것이 아니라면 뒤에 'or None'을 붙여줌(여기서는 달아줌)  
실행하여 새글쓰기를 하여보고 관리자 페이지에서 확인하면 이미지 업로드가 된 것을 확인 할 수 있음(생략)  
수정도 정상적을 되는 것을 확인 할 수 있음(생략)  
  
해결방법정리  
#### (1) form에서 enctype="multipart/form-data" 설정  
#### (2) request.FILES로 이미지 객체 정보 담아오기  
  
## 2. 예시(main 페이지에서 업로드한 이미지를 띄어보기)  
1. bootstarp > 'card' 검색 > Example 코드에서 아래의 코드를 복사하여  
   posts 앱(폴더) > templates 폴더 > posts 폴더 > main.html에 아래와 같이 붙여넣기 및 수정  
![main수정](https://user-images.githubusercontent.com/31130917/107259782-c5b0f980-6a80-11eb-8bc1-ec07930de8a4.PNG)  
실행하면 에러발생  
=> 이미지가 없는 게시글도 있기 때문  
  
2. 조건문 설정  
아래와 같이 코드 추가  
![main수정2](https://user-images.githubusercontent.com/31130917/107260102-20e2ec00-6a81-11eb-98ac-6cd873642df7.PNG)  
실행하면 아래와 같은 결과를 확인 할 수 있음  
![main수정2결과](https://user-images.githubusercontent.com/31130917/107260248-4839b900-6a81-11eb-82f0-c81788e3f083.PNG)  
  
3. 기본 이미지 설정하기  
media 폴더 > posts 폴더 > default 폴더 만들고 기본 이미지 저장  
posts 앱(폴더) > models.py에서 image 아래의 코드 추가 및 수정  
<pre><code>
image = models.ImageField(upload_to="posts/img",
                              default="posts/default/default_post_img.PNG")  
...
</code></pre>  
  
4. 변경사항에 대해서 주문서 만들고 반영  
  
5. 관리자 페이지에서 기존에 이미지가 없던 게시글은 모두 삭제  
새로 글을 생성하고(이때 이미지 업로드 안함) 관리자 페이지를 보면 기본 이미지가 업로드된 것을 확인 할 수 있음(생략)
