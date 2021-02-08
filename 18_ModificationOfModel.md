# 18강. Model의 수정과 조회수의 구현  

## 1. 예시(한국어로 바꾸기)  
#### 1. 프로젝트 폴더 > settings.py에서 'LANGUAGE_CODE'를 'ko-kr'로 바꿈  
실행하여 admin 페이지에 들어가면 한국어로 바뀐 것을 확인 할 수 있음(생략)  
하지만 column이 바뀐 것은 아님  
  
2. column까지 한국어로 바꾸기  
posts 앱(폴더) > models.py 에서 아래와 같이 코드 추가  
<pre><code>
...  
    title = models.CharField(max_length=300, verbose_name="제목")  
    content = models.TextField(verbose_name="내용")  
    view_count = models.IntegerField(default=0, verbose_name="조회수")  
    created_at = models.DateTimeField(auto_now_add=True, verbose_name="등록시간")  
    updated_at = models.DateTimeField(auto_now=True, verbose_name="업데이트시간")  
...
</code></pre>  
#### Django 모델에서 속성을 변화시켜준 것이므로 바로 반영 됨  
  
#### 3. 위와 같이 변경사항이 있을 경우 실제 데이터베이스에 적용시킬 수 있음  
터미널에 아래의 코드를 작성하여 주문서 만듬  
<pre><code>
# python manage.py makemigrations
</code></pre>  
자동으로 migration이 만들어진 것을 확인 할 수 있음(주문서에는 변경한 내용들이 담겨져 있음)  
  
아래의 코드를 입력하여 데이터베이스에 실제로 변경한 내용들을 반영  
<pre><code>
# python manage.py migrate
</code></pre>  
  
## 2. 예시(게시글에 type 추가하기)  
1. 특정 데이터만 선택할 수 있는 제약적인 조건 만들기  
posts 앱(폴더) > models.py 에서 아래와 같이 코드 추가(튜플 작성)  
<pre><code>
    ordering ...  
POST_TYPES = [  
    (0, "칼럼"),  
    (1, "뉴스"),  
    (2, "소설")  
]  
// POST_TYPES 라는 리스트 안에 튜플로 작성  
// 변경되는 값이 아니기 때문에 튜플로 작성할 수 있음
</code></pre>  
  
2. 양의 작은 정수만 뽑기  
아래와 같인 코드 추가  
<pre><code>
content ...  
_type = models.PositiveSmallIntegerField(choices=POST_TYPES, verbose_name="게시글타입")  
</code></pre>  
#### 실제 데이터는 숫자만 들어가고 사용자가 보는 부분에서는 문자열로 보임  
  
3. 변경사항이 발생하였으므로 주문서를 만들어 데이터베이스에 변경한 내용들을 반영  
=> migration을 만드는 명령을 입력하였을 때 에러발생  
### 컬럼은 기본적은로 null=false(컬럼값이 비어있으면 안됨)가 붙음  
### 새로운 컬럼을 추가하려 할 때 컬럼의 기존의 객체에 이 컬럼에 해당하는 데이터 값이 없어서 만들 수가 없는 상황  
=> Django에서는 2가지 방법을 제시  
    1. 기존의 객체들에게 기본값(default)를 부여하는 방법  
    2. 종료를 하고 default 값이나 null=true 옵션을 넣어줌  
#### 여기서는 0을 선택하여 기본값으로 컬럼을 넣어줌  
#### 1번의 과정을 통해 주문서에 기본값(0)이 들어가게 됨  
  
### 변경사항을 데이터베이스에 반영할 때 절대로 주문서는 건들지 않음(남들과 협업할 때는 특히, 물론 혼자할 때도 안됨)  
### 항상 변경사항은 models.py -> makemigrate -> migrate  
  
4. migrate 해줌  
posts 앱(폴더) > admin.py에서 'list_display'의 'id' 밑에 '_type' 추가  
  
실행 후 관리자 페이지에 들어가면 컬럼에 게시글타입이 추가된 것을 확인할 수 있음(생략)  
post 추가시에도 게시글 타입을 선택할 수 있음(생략)  
  
## 3. 예시(조회수 기능 구현)  
구현 방법은 굉장히 다양(그중에서 심플한거)  
  
1. posts 앱(폴더) > templates 폴더 > posts 폴더 > show.html에서 아래의 코드 추가  
![조회수구현](https://user-images.githubusercontent.com/31130917/107214073-b2843680-6a4c-11eb-89a2-e5a3774d42c7.PNG)  
  
2. posts 앱(폴더) > views.py에서 아래의 코드 추가  
<pre><code>
def show(request, post_id):  
    post = get_object_or_404(Post, pk=post_id)  
    default_view_count = post.view_count // 추가 작성 부분  
    post.view_count = default_view_count + 1 // 추가 작성 부분  
    post.save() // 추가 작성 부분  
    context = {  
        'post': post  
    }  
    return render(request, 'posts/show.html', context)
</code></pre>  
실행하면 아래와 같은 결과를 확인할 수 있음(새로고침 할 때마다 조회수 올라감)  
![조회수결과](https://user-images.githubusercontent.com/31130917/107214442-435b1200-6a4d-11eb-8c62-5fc178866179.PNG)
