# Django Self Study

```shell
    source myvenv/bin/activate
```
- 분리한 개발환경을 실행하는 부분 


```python
    from django.conf import settings
```
- from or import로 시작하는 부분은 다른 파일에 있는 것을 추가하라는 의미
   - 매번 다른 파일에 있는 것을 복사 & 붙여넣기로 해야 하는 작업을 from이 대신 불러와 주는 것 

```python
    class Post(models.Model)
```
- 모델을 정의하는 코드 
- class : 특별한 키워드, 객체를 정의한다는 것을 알려줌 
- Post : 모델의 이름, 클래스 이름의 첫 글자는 대문자로 작성해야 함 
- models : Post가 장고 모델임을 의미 
    - 이 부분 덕분에 장고는 Post가 DB에 저장되어야 한다고 알게 됨 

- 속성을 정의하는 것
    - 필드마다 어떤 종류의 데이터 타입을 가지는지를 정해야 함 
    - 여기서 데이터 타입에는 텍스트, 숫자, 날짜, 사용자 처럼 다른 객체 참조가 존재 
    - models.CharField : 글자 수가 제한된 텍스트를 정의할 때 사용 
    - models.TextField : 글자 수에 제한이 없는 긴 텍스트를 위한 속성 
    - models.DateTimeField : 날짜와 time을 의미 
    - models.ForeignKey : 다른 모델에 대한 링크를 의미  

- def publish(self) : publish 라는 메서드 
    - def : 함수, 메서드라는 의미 

- 메서드는 자주 무언가를 되돌려줌 
    - __str__ 메서드를 보면, 호출하면 Post 모델의 제목 텍스트를 얻게 됨 

# Django 모델 

- 어플리케이션 => 블로그 글 모델 제작 => 데이터베이스에 모델을 위한 테이블 제작

- Django는 데이터베이스에 지금 반영할 수 있도록 마이그레이션 파일이라는 것을 준비해 둠 
```shell
    python manage.py migrate blog
```
- 실제 데이터베이스에 모델 추가를 반영 => 글 모델이 데이터베이스에 저장됨 

# Django 관리자

- LANGUAGE_CODE = 'en-us' => 'ko'로 변경 : 관리자 화면을 한국어로 변경 가능

- 모델링한 글들을 관리자에서 추가, 수정, 삭제 가능 

```python
    from django.contrib import admin
    from .models import Post

    admin.site.register(Post)
```
- 앞 장에서 정의했던 Post 모델을 가져오고 있음 
- 관리자 페이지에서 만든 모델을 보려면 `admin.site.register(Post)`로 모델을 등록해야 함 

- 로그인 하기 위해선, 모든 권한을 가지는 슈퍼 사용자가 필요 
```python
    python manage.py createsuperuser
```

# 배포 
- 애플리키에션을 인터넷에 올려 다른 사람들도 볼 수 있게 하는 것 
- pythonAnywhere를 사용할 것 

- db.sqlite3 : 모든 게시물이 저장된 로컬 데이터베이스
    - pythonAnywhere는 다른 데이터 베이스를 사용하기 때문에 저장소에 추가될 필요 X
    - 다른 데이터베이스로는 SQLite로도 사용하지만 보통은 SQLite보다 휠씬 많은 방문자를 보유한 웹사이트의 경우 => MySQL을 사용함 
    - GitHub 저장소에 SQLite 데이터베이스를 제외하고 저장하면, 지금까지 작성한 모든 게시물을 로컬에서만 사용 가능 
        - 다시 새 데이터베이스를 추가해야 함 
    - 로컬 데이터베이스는 데이터가 삭제되어도 괜찮은 테스트 공간으로만 사용하자


# pythonAnyware 
- git clone으로 가져와 사용

- 가상환경을 생성 가능 
```shell
virtualenv --python=python3.6 myvenv

source myvenv/bin/activate

pip install django~=2.0
```

- pythonAnywhere에서 DB 생성 
    - 로컬과 서버가 다른점 : 다른 데이터베이스를 사용한다 
    - 사용자 계정과 글은 서버와 로컬이 다를 수 있음
    - 서버에서도 데이터베이스 초기화가 필요  
    ```shell
        python manage.py migrate

        python manage.py createsuperuser
    ```

- web app으로 블로그 배포 
    - 코드는 pythonAnywhere에 존재, 가상환경도 준비됨 
    - 정적 파일들도 모여 있고, 데이터베이스도 초기화 됨 => 웹 앱으로 배포할 준비완료
    - Web 탭에서 새로 만들면 되는데, 수동설정에서 Python 3.6만 주의
    Virtualenv: 부분에 가상환경 추가해서 설정하기 

- Django는 WSGI 프로토콜을 사용해 작동
    - 파이썬을 이용한 웹 사이트를 서비스하기 위한 표준 
    - /var/www/imhojeong_pythonanywhere_com_wsgi.py
    - 이 파일 찾아서 바꾸어 주면 됨 
    ```

- pythonAnywhere에게 웹 애플리케이션의 위치와 Django 설정 파일명을 알려주는 역할
    - StaticFilesHAndler : CSS를 다루기 위한 것 
    - runserver 명령으로 로컬 개발 중에 자동으로 처리됨 


- 사이트의 기본 주소에 /admin을 붙이면 관리자 사이트로 이동함 
    - 사용자 이름, 암호로 로그인해 서버에 새 게시물 추가 가능 

    - 로컬에서 변경하고, 변경 사항을 Github에 적용하고 변경 사항을 실제 웹 서버로 가져옴 
        - 이를 통해 실제 웹 사이트를 손상시키지 않고 작업, 테스트 가능 

# URL
- 웹 주소, 인터넷의 모든 페이지는 고유한 URL을 가지고 있어야 함
- 애플리케이션은 사용자가 URL을 입력하면 어떤 내용을 보여주어야 하는지 알고 있음
- 장고 : URLconf(URL configuration)을 사용
- URLconf : 장고에서 URL과 일치하는 뷰를 찾기 위한 패턴들의 집합
- mysite/urls.py에서 알 수 있음 
    - 세 개의 따옴표들 사이에 있는 줄들은 docstring
    - docstring : 파일 제일 첫 부분, 클래스 or 메서드 윗 부분에 작성해, 이들이 어떤 일을 수행하는지 알려줌 
        - 파이썬은 이 부분을 실행하지 않을 것
        - 관리자 URL도 여기에 이미 있음 
- Django : admin/로 시작하는 모든 URL을 view와 대조해 찾아냄
    - 무수히 많은 URL이 admin URL에 포함될 수 있어 => 일일이 모두 쓸 수 없음 => 정규표현식을 사용함

- http://127.0.0.1:8000 주소를 블로그 홈 페이지로 지정하고 글 목록을 보여주자
    - mysite/urls.py 파일을 깨끗한 상태로 유지하기 위해 blog 애플리케이션에서 메인 mysite/urls.py 파일로 url들을 가져올 것

    ```python
        from django.contrib import admin
        from django.urls import path, incldue 

        urlpatterns = [
            path('admin/', admin.site.urls),
            path('', include('blog.urls'))
        ]
    ```
    - http://127.0.0.1:8000/로 들어오는 모든 접속 요청을 blog.urls로 전송해 추가 명령을 찾을 것

- blog.urls
```python
from django.urls import path
from . import views
```
- 장고 함수인 path와 blog 애플리케이션에서 사용할 모든 view를 가져옴

- 첫 번째 URL 패턴을 추가
    ```python
        urlpatterns = [
            path('',views.post_list, name='post_list')
        ]
    ```
    - post_list라는 view가 루트 URL에 할당됨 
    - 이 URL 패턴은 빈 문자열에 매칭되고, 장고 resolver는 전체 URL 경로에서 접두어에 포함되는 도메인 이름(ex - http://127.0.0.1:8000)을 무시하고 받아들임

    - 장고에게 누군가 웹사이트에 http://127.0.0.1:8000/ 주소로 들어왔을 때 views.post_list를 보여주라고 말해줌 
    - name = 'post_list' : URL에 이름을 붙인 것으로 뷰를 식별 => 뷰의 이름과 같을 수도 완전히 다를 수도 있음 
    - 이름을 붙인 URL은 프로젝트의 후반에 사용할 것 => 앱의 각 URL마다 이름짓는 것은 중요
    - URL에 고유한 이름을 붙여, 외우고 부르기 쉽게 만들어야 함

- http://127.0.0.1:8000 => 웹 페이지를 사용할 수 없음?
	- 서버가 실행되지 않았기 때문 
	- 콘솔에서 에러가 발생한 경우, no attribute 'post_list' ?
	- 장고가 찾고 사용하고자 하는 뷰가 아직 없다는 것 => /admin/도 접속되지 않을 것 
	- 웹 서버는 Ctrl+c로 중단, python manage.py runserver 명령어를 실행해 서버를 다시 시작

---

# Django View

- View? 애플리케이션의 로직을 넣는 곳, 이전 장에서 만들었던 모델에서 필요한 정보를 받아와 템플릿에 전달하는 역할을 함 
	- views.py 파일 안에 있음

```python
def post_list(request):
    return render(request, 'blog/post_list.html',{})
```
- 요청(request)를 넘겨받아 render 메서드를 호출
- render 메서드를 호출해 받은 blog/post_list.html 템플릿을 보여줌 
- 접속해 확인해 보면 TemplateDoesNotExist 에러 확인 가능 => 템플릿 파일이 없기 때문에 


# 템플릿 

- 서로 다른 정보를 일정한 형태로 표시하기 위해 재사용 가능한 파일 
- 장고 템플릿 양식은 HTML을 사용
- blog/templates/blog 디렉토리에 저장됨 => 이곳에 post_list.html 파일 생성 => 이 안에 내용을 추가해서 작성하면 됨 

- github에 push 후 pythonanywhere에서 pull 해서 업데이트 해보자

---

# Django ORM => QuerySets
- QuerySet : 전달받은 모델의 객체 목록 

- Django interactive console 
```python 
    python3 manage.py shell
```
- 파이썬 프롬프트와 비슷하지만, 장고만의 마법을 부릴 수 있음 
    - 파이썬의 모든 명령어를 여기서 사용할 수 있음 

## 모든 객체 조회

```shell
>> from blog.models import
>> Post.objects.all()
```
- Post 모델을 blog.models에서 불러온 것 
    - 게시된 글 목록이 나타남, python으로 새 글을 포스팅하려면?

## 객체 생성 
- DB에 새 글 객체를 저장하는 방법에 대해 알아보자
```python
>>> Post.objects.all()
```
- 작성자로서 User 모델의 인스턴스를 가져와 전달해주어야 한다?
    - 그리고 게시물을 생성 
```python
>>> from django.contrib.auth.models import User
>>> me = User.objects.get(username='admin')

>>> Post.objects.create(author=me, title='Sample title', text='Test')
>>> Post.objects.all() # 이것으로 게시글 종류 확인
```

## 필터링하기
- Queryset의 중요한 기능 => 데이터를 필터링하는 것 
- 특정 사용자가 작성한 모든 글을 찾고 싶다??
    - Post.objects.all => Post.objects.filter()를 사용
    - 쿼리셋 안에 있는 괄호 안에 원하는 조건을 넣어주자 
        - 지금 이 경우엔, author가 me인 조건을 넣어야 함 
```python
>>> Post.objects.filter(author=me)
```
- 모든 글들 중, title에 title이라는 글자가 들어가는 글들만을 뽑아내서 보고 싶다면?
- title__contains 부분에서 사이에 있는 밑줄 _가 2개 
    - 장고 ORM은 필드 이름(title)과 연산자와 필터("contains")를 밑줄 2개를 사용해 구분함 
         - 1개만 입력한다면, FieldError: Cannot resolve keyword title_contains라는 오류 발생
```python
>>> Post.objects.filter(title__contains='title')
```

- 게시글 목록을 볼 수 있나? => published_date로 과거에 작성한 글을 핕터링하면 목록을 불러올 수 있음 
```python
>>> from django.utils import timezone
>>> Post.objects.filter(published_date__lte=timezone.now())
```

- 게시하려는 게시물의 인스턴스를 얻어야 함
```python
>>> post = Post.objects.get(title="Sample title")
```
- publish 메소드를 사용해 게시 ㄱㄱ
```python
>>> post.publish()
```


## 정렬하기 
- QuerySet은 객체 목록을 정렬 가능 => created_date 필드를 정렬해보자
- `-`를 맨 앞에 붙여주면 내림차순 정렬 가능 
```python
>>> Post.objects.order_by('created_date')
>>> Post.objects.order_by('-created_date')
```

## 쿼리셋 연결도 가능 
- QuerySet들을 연결 가능 
```
>>> Post.objects.filter(published_date__lte=timezone.now())
.order_by('published_date')
```
- 정말 복잡한 쿼리도 작성할 수 있게 해줌 


## 템플릿 동적 데이터 

- 블로그 글은 각각 다른 장소에 조각조각 나누어져 있음 
- Post 모델 => models.py
- post_list 모델 => views.py
- 앞으로 템플릿도 추가해야 함 
    - HTML 템플릿에서 글 목록을 어떻게 보여줄 것?
    - 콘텐츠를 가져와 템플릿에 넣어 보여주는 것을 해보자 
        - 콘텐츠(데이터 베이스 안에 저장되어 있는 모델)

- view : 모델과 템플릿을 연결하는 역할을 함
    - post_list를 뷰에서 보여주고 이를 템플릿에 전달하기 위해선 모델을 가져와야 함 
    - 뷰가 템플릿에서 모델을 선택하도록 만들어야 함
    - 다른 파일에 있는 코드를 어떻게?
        - models.py 파일에 정의된 모델을 가져올 것 
```python
from .models. import Post 
```
- from 다음에 있는 . 은 현재 디렉토리 or 애플리케이션을 의미 
- 동일한 디렉터리 내 views.py, models.py 파일이 있기 때문에 .파일명 => .py 확장자를 붙이지 않아도 됨 => 내용을 가져올 수 있음 

- Post 모델에서 블로그 글을 가져오기 위해선 QuerySet이 필요함 

## Query Set
- 글 목록을 게시일 기준으로 정렬?
- timezone 모듈을 불러와서 정렬해야 함
```python
from django.utils import timezone

Posts.objects.filter(published_date__lte=timezone.now()
.order_by('published_date'))
```
- posts Queryset을 템플릿 컨텍스트에 전달하는 것이 필요함 

- posts 변수 : 쿼리셋의 이름 
    - posts 쿼리셋을 템플릿에 보내는 방법?

- render 함수 : 매개변수 request & 'blog/post_list.html' 템플릿이 있음 
    - request : 사용자가 요청하는 모든 것
    - {} : 이곳에 템플릿을 사용하기 위해 매개변수를 추가할 필요가 있음 
        - { 'posts' : posts } 처럼
        - : 이전에 문자열이 와야 하며, 작은 따옴표를 양쪽에 붙여야 함 


## 장고 템플릿 
- 데이터를 보여주기에 유용한 기능은 템플릿 태그가 존재 => template tags

- 템플릿 태그 
    - HTML에 파이썬 코드를 바로 넣을 수 없음 
        - 브라우저는 파이썬 코드를 이해할 수 없음
    - 브라우저는 HTML만을 알고 있음, HTML은 정적이지만, 파이썬은 동적임 

- post 목록 템플릿 보여주기 
    - 글 목록이 들어있는 posts 변수를 템플릿에 넘겨 주었다면 
    - posts 변수를 받아 HTML에 나타나도록 해보자
    - 장고 템플릿 안에 있는 값을 출력
        - 변수 이름 안에 중괄호를 넣어 표시해야 함
    ```html
    {{ posts }}
    ```
    - {{ posts }} : 이것을 객체 목록으로 이해하고 처리했다는 것을 의미 
    ```html
    {% for post in posts %}
        {{ post }}
    {% %}
    ```

    - 만들고 나면 디자인이 별로일 수 있음 
        - 정적 블로그 게시글들이 보이게 만들면 참 좋을텐데....
        - HTML과 템플릿 태그를 섞어 사용하면 멋있게 만들 수 있음 
    ```html
  <div>
    <h1><a href="/">Django Girls Blog</a></h1>
</div>

{% for post in posts %}
    <div>
        <p>published: {{ post.published_date }}</p>
        <h1><a href="">{{ post.title }}</a></h1>
        <p>{{ post.text|linebreaksbr }}</p>
    </div>
{% endfor %}
    ```
- {% for ~~~ %} 와 {% endfor %} 사이에 넣은 모든 것은 목록의 모든 객체를 반복하게 됨 
- {{ post.title }} => 이전과 다른 표기법
    - Post 모델에서 정의한 각 필드의 데이터에 접근하기 위해 이 표기법을 사용함 
    -  | linebreaksbr => 파이프 문자도 사용함 
    - 블로그 글 텍스트에서 행이 바뀌면 문단으로 변환하도록 하라는 의미 
    - 행바뀜을 문단으로 변환하는 필터를 적용한다는 표현을 쓰기도 함

- PythonAnywhere 사이트의 블로그 게시물이 로컬 서버에 호스팅 된 블로그에 나타나는 게시물과 일치하지 않는 것이 정상 
    - 로컬 컴퓨터와 PythonAnywhere의 데이터베이스는 동기화 되지 않음 

# CSS => BootStrap 추가
- BootStrap : HTML, CSS 프레임워크 -> 예쁜 웹 사이트 제작 가능 

- 설치 
    - '<head>'에 링크만 넣어주면 됨
    - 프로젝트에 인터넷에 있는 파일을 연결하는 것 

## 정적 파일 
- CSS, 이미지 파일에 해당 
- 요청 내용에 따라 바뀌는 것이 아니라 모든 사용자들이 동일한 내용을 볼 수 있음

- 정적 파일은 어디에?
    - 서버에서 collectstatic을 실행할 때 처럼, Django는 admin 앱에서 
    정적 파일을 어디서 찾아야하는지 이미 알고 있음 
    - blog 앱에 정적파일을 추가하면 됨 
    - blog => static 폴더를 만들자
    - Django는 app 폴더 안에 있는 static 폴더를 자동으로 찾을 수 있음 => 이 컨텐츠를 정적 파일로 사용하게 되는 것

- static 디렉토리 => css => blog.css를 만들어보자 
    - #으로 시작해 알파벳과 숫자 중 6개를 조합해 hexacode로 나타냄
    - CSS 파일에선 HTML 파일에 있는 각 요소들에 스타일 정의 가능 
    - css를 HTML에 추가해 보자 => 맨 처음 줄에 이 라인을 추가하자
    ```html
    {% load static %}
    ```
    - 위의 부분에 정적 파일을 로딩하는 것 
    - 다음 head와 /head 사이에 Bootstrap css 파일 링크 다음에 추가
    ```html
    <link rel="stylesheet" href="{% static 'css/blog.css' %}">
    ```
    - 브라우저는 주어진 순서대로 파일을 읽음 
        - 파일이 올바른 위치에 있는지 확인해야 함
        - 파일 코드가 부트 스트랩 파일의 코드를 무시할 수 있는 것을 막기 위해 
    - 폰트 변경 링크
        - google 글꼴에서 lobster라는 글꼴을 가져온 것을 말해
    ```html
    <link href="//fonts.googleapis.com/css?family=Lobster&subset=latin,latin-ext" rel="stylesheet" type="text/css">
    ```

## 템플릿 확장하기 

- 우리 웹 사이트 안의 서로 다른 페이지에서 HTML의 일부를 동일하게 재사용할 수 있다는 뜻
- 동일한 정보/레이아웃을 사용하고자 할 때, 모든 파일마다 같은 내용을 반복해서 입력할 필요가 없게 됨 
    - 무언가 수정할 부분이 생겼을 때 딱 한번만 수정하면 됨 

- 기본 템플릿 생성
    - 웹 사이트 내 모든 페이지에 확장되어 사용되는 가장 기본적인 템플릿
    - blog/templates/blog/ => base.html
    ```html
        {% block content %}
        {% endblock %}
    ```
    - body의 주요 부분을 위처럼 변화 
        - `{% for post in posts%} ~ {% endfor %}` 사이에 있는 모든 내용을 바꿈 
    - `{% block content %} ~ {% endblock %}`
        - block을 만든 것 => HTML 내에 들어갈 수 있는 공간을 만듬 
        - base.html을 확장해 다른 템플릿에도 가져다 쓸 수 있게 한 것 
        - post_list.html의 내용을 for문 제외하고 다 삭제 
    - why? 이 코드를 모든 컨텐츠 블록에 대한 템플릿의 일부로 작성할 예정 => 이 파일에 블록 태그를 추가
    - 주의! 블록 태그가 base.html 파일의 태그와 일치해야 함 & 콘텐츠 블록에 속한 모든 코드를 포함하게 만들어야 할 것 
        - `{% block content %} ~ {% endblock %}` 안에 넣어두자
    - 두 개의 템플릿을 연결해야 함 
        - 확장 태그를 파일 맨 처음에 추가
        - `{% extends 'blog/base.html' %}`


