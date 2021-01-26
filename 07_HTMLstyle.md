# 7강. 모든 HTML에 스타일 적용하기

## 1. 예시  
1. BlackBoard(작성자가 만든 메인 폴더) 폴더 안의 templates 경로를 setting.py에 추가  
TEMPLATES의 DIRS에 코드 추가  
<pre><code>
'DIRS': [os.path.join(BASE_DIR, 'BlackBoard', 'templates')],
</code></pre>  
BlackBoard 폴더 안에 'templates'라는 경로를 읽어올 수 있음  
  
2. BlackBoard 폴더 > templates 폴더 > base.html 만듬  
'bootstrap'사이트에 들어가 base.html에 넣을 뼈대 html 코드 카피  
head의 title 부분은 'My First Django'로 바꾸고 body의 'hello world!'는 지움  
지운 부분에 아래와 같은 코드 추가  
<pre><code>
{% block content %}  
{% endblock %}
</code></pre>  
기존 뼈대(base.html)을 가져오고 변경사항만 block으로 들어감  
  
3. main.html에서 base.html 가져오기  
posts 앱(폴더)의 main.html에서 다음 코드를 추가 작성  
![html뼈대](https://user-images.githubusercontent.com/31130917/105851637-b369a100-6026-11eb-9b8c-e304d38ef70f.PNG)  
settings.py에 1번의 코드를 작성하여 경로를 잡아주었기 때문에 main.html에서 바로 base.html을 가져올 수 있음  
또한 block코드를 작성하여 html코드가 뼈대 안으로 들어가게 함  
실행하면 다음과 같은 결과창이 나타남 전과 다르게 스타일이 살짝 바뀐 것을 확인 할 수 있음  
![결과](https://user-images.githubusercontent.com/31130917/105851852-fb88c380-6026-11eb-99a7-5a78da403b9e.PNG)  
  
메인 창만 스타일이 바뀌었기 때문에 링크페이지도 바뀌게 하려면 해당 html에 들어가 뼈대를 만들어야함  
만을 다른 뼈대를 만들어야 한다면 그 html파일이 들어있는 폴더 안에 base.html파일을 만들어 뼈대를 만듬  
main.html처럼 만들어줌  
다시 실행 후 들어가면 스타일이 바뀐 것을 확인 할 수 있음  

## 2. 모든 html에 스타일 적용  
1. settings.py에 들어가 STATIC_URL 밑에 아래와 같이 코드 추가  
<pre><code>
STATICFILES_DIRS = [  
    os.path.join(BASE_DIR, 'BlackBoard', 'static')  
]
</code></pre>  
  
2. BlackBoard 폴더 > static 폴더 > css, js, font등 폴더 만듬  
css 폴더 > project.css 만듬  
  
3. css의 stylesheet 가져오기  
BlackBoard 폴더의 base.html에 아래의 코드를 추가 작성  
![css](https://user-images.githubusercontent.com/31130917/105854308-16a90280-602a-11eb-9ef5-c9644781d102.PNG)  
  
template 태그에서 static 쓰려면 load static 필요  
base.html 코드 맨위에 아래의 코드 추가 작성  
<pre><code>
{& load static %}
</code></pre>  
  
project.css에는 아래와 같이 코드 작성  
<pre><code>
h1 {  
    font-size: 50px;  
}
</code></pre>  
실행하면 아래와 같이 글씨 크기가 바뀐 것을 확인 할 수 있음  
![폰트사이즈변경결과](https://user-images.githubusercontent.com/31130917/105854935-d5652280-602a-11eb-9fa8-77933f9fd69f.PNG)  
링크페이지도 모두 변경된 것을 확인 할 수 있음(생략)
