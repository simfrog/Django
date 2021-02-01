# 10강. CRUD란?  

## 1. 개념  
### create, read, update, delete  
### 대부분의 소프트웨어가 가지는 기본 데이터 처리 기능들  
ex) 전화번호, 주소록  
#### admin page에서 구성  
#### ORM(Object Relational Mapping)을 통해 python과 SQL사이에서 통역사 역할  
  
## 2. ORM 예시  
* Create(생성하기)  
<pre><code>
Post.objects.create(title="간장게장 담그는법", content="맛있는 꽃게를 준비한다...")
</code></pre>  
  
* Read(읽기)  
<pre><code>
Post.objects.all() // Post 객체 모두 불러오기  
Post.objects.first() // Post에서 첫번째 객체 불러오기  
Post.objects.get(id=3) // Post에서 id가 3인 객체 불러오기
</code></pre>  
  
* Update(수정하기)  
<pre><code>
post = Post.objects.get(id=3)  
post.title = "간장게장 맛있게 담그는 법"  
post.save()
</code></pre>  
  
* Delete(삭제하기)  
<pre><code>
post = Post.objects.get(id=3)  
post.delete()
</code></pre>
