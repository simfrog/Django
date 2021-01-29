# 9강. STATIC 개념과 정적 이미지 삽입  

## 1. 개념
* ### static 파일 : 변하지 않는 파일 (css, js, image 등)  
* 사용방법  
  1. Static 파일(정적 파일) 들을 한곳에 모은다.  
  ![static폴더](https://user-images.githubusercontent.com/31130917/106287789-92a18580-628a-11eb-9629-10f047d7b98e.PNG)  
  2. 장고에게 정적파일의 경로를 알려준다.(settings.py에 아래의 코드를 작성하여 알려줌)  
  <pre><code>
  STATICFILES_DIRS = [
    os.path.join(BASE_DIR, 'BlackBoard', 'static')
  ]
  </code></pre>  
  base.html에서 '{% static 'static 경로' %} 사용하려면 맨 위에 '{% load static %}' 작성  
  
## 2. 예시  
1. 다운받은 이미지를 정적파일 경로 img 폴더 안으로 가져옴  
2. 이미지를 삽입할 html 파일(여기서는 main.html에 올릴 계획)에 아래와 같은 코드 추가 작성  
![이미지 삽입](https://user-images.githubusercontent.com/31130917/106289047-2162d200-628c-11eb-8fcd-6824ac6ffb46.PNG)  
(alt는 만약 사진을 가져오지 못했을 경우 공백 출력하라는 뜻)  
### static 사용시 load static 꼭 붙이기  
실행하면 아래와 같은 결과를 확인 할 수 있음  
![결과창](https://user-images.githubusercontent.com/31130917/106289240-6129b980-628c-11eb-8c36-c34c4e4b5787.PNG)
