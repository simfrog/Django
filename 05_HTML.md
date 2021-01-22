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
