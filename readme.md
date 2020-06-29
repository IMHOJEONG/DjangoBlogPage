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


## 애플리케이션 확장 
- 블로그 게시글이 각 페이지마다 보이게 만들어 보자, 이미 앞에서 Post 모델을 만들었으니 models.py에 새로 추가할 내용은 없음 

```html
<h1><a href="{% url 'post_detail' pk=post.pk %}">{{ post.title }} </a></h1>
```
- 이 코드를 위처럼 수정해서 Post 제목 목록이 보이고 해당 링크를 클릭하면 post 상세 페이지로 이동하게 만듬 
- `{% url 'post_detail' pk=post.pk %}` : `{% %}` - 장고 템플릿 태그 => URL을 생성해 사용해보자
    - blog.views.post_detail : post_detail 뷰 경로 
    - `blog은 응용프로그램 (디렉터리 blog)의 이름인 것!`
    - views : views.py 의 파일명 -> post_detail은 view 이름 

- pk = post.pk : pk - 데이터베이스의 각 레코드를 식별하는 기본키(Primary key)의 줄임말 
    - post 모델에서 기본키를 지정하지 않았기 때문에 Django는 pk라는 필드를 추가해 새로운 블로그 게시물이 추가될 때마다 그 value가 1,2,3 등으로 증가하게 됨 
    - Post 객체의 다른 필드에 액세스하는 것과 같은 방식으로 post.pk를 작성해 기본키에 액세스
    - post.pk를 써서 기본키에 접근 가능 & Post 객체 내 다른 필드(title, author)에도 접근 가능 

## Post 상세 페이지 URL 만들기 
- post_detail view가 보이게 urls.py에 URL을 만들어야 함 
    - blog/urls.py 파일에 URL을 만들어 장고가 post_detail 뷰로 보내, 게시글이 보일 수 있게 해보자 
    - `path('post/<int:pk>/', views.post_detail, name='post_detail')` 코드를 추가 
    - `post/<int:pk>` : URL 패턴을 나타냄 
        - post/ : URL이 post문자를 포함해야 한다는 것을 말함 
        - `<int: pk>` : 장고는 정수value를 기대하고 이를 pk라는 변수로 뷰로 전송하는 것을 의미 
        - `/` : 다음에 /가 한 번 더 와야 한다는 의미
    - ex) http://127.0.0.1:8000/post/5
        - 장고는 post_detail 뷰를 찾아 매개변수 pk가 5인 value를 찾아 뷰로 전달 

## Post 상세 페이지 내 뷰 추가하기 
- 뷰에 매개변수 pk를 추가해보자 -> 뷰가 pk를 식별해야 함 
```python
    def post_detail(request, pk):
```
- urls(pk)와 동일하게 이름을 사용해야 함 => 변수가 생략되면 오류가 발생 
- 블로그 게시글 한 개만 보려면? 
```python
Post.objects.get(pk=pk)
```
- 하지만 위 코드에는 문제가 존재 => 만약 해당 primary key의 Post를 찾지 못하면 오류가 발생
- Django에선 이를 해결하기 위해 get_object_or_404라는 특별한 기능을 제공 
    - pk에 해당하는 Post가 없을 경우 : 페이지를 찾을 수 없음 404 : Page Not Found 404를 보여줄 예정

- 나중에 Page not found 페이지를 예쁘게 만들 수 있음 

- views.py 파일에 새로운 뷰를 추가하자
    - blog/urls.py 파일에서 views.post_detail라는 뷰를 post_detail 이라 이름을 붙이도록 URL 법칙을 만듬 
        - Django가 post_detail이라는 이름을 해석할 때, blog/views.py 파일 내부의 post_detail이라는 뷰 함수로 이해하도록 해줌 
    ```python
    from django.shortcuts import render, get_object_or_404
    ```
    - 위의 모듈 import하고 파일 마지막 부분에 뷰를 추가 
    ```python
    def post_detail(request, pk):
        post = get_object_or_404(Post, pk=pk)
        return render(request, 'blog/post_detail.html', { 'post' : post})
    ```
     
## Post 상세 페이지 템플릿 만들기
- blog/templates/blog 디렉토리 안 post_detail.html이라는 새 파일 생성 후 작성
```html
{% extends 'blog/base.html' %}

{% block content %}
    <div class="post">
        {% if post.published_date %}
            <div class="date">
                {{ post.published_date }}
            </div>
        {% endif %}
        <h1>{{ post.title }}</h1>
        <p>{{ post.text|linebreaksbr }}</p>
    </div>
{% endblock %}
```
- base.html을 확장한 결과, content 블록에서 블로그 글의 게시일, 재목과 내용을 보이게 만듬 

- 가장 중요한 부분 `{% if ... %} ~ {% endif %}`라는 템플릿 태그
    - 내용이 있는지 확인할 때 사용함 
    - 위의 경우 : post의 게시일이 있는지 없는지 확인하는 것

## Django Form
- Blog 글 추가 및 수정하는 것 & 폼으로 엄청난 인터페이스를 만들 수 있음 
- 아무런 준비 없이도 양식 만들 수 있고 ModelForm을 생성해 자동으로 모델에 결과물을 저장할 수 있다는 것
- 폼을 하나 만들어서 Post 모델에 적용
- 장고의 모든 중요한 부분과 마찬가지로, 폼도 폼만의 forms.py라는 파일을 만들어짐 
- blog - forms.py
```python
from django import forms 

from .models import Post

class PostForm(forms.ModelForm):

        class Meta:
            model = Post
            fields = ('title', 'text',)
```
- forms model을 import 해야 하고 model도 import 해야 함 
- PostForm : 우리가 만들 폼의 이름 -> Django에 이 폼이 ModelForm이라는 것을 알려주자 
    - forms.ModelForm : ModelForm이라는 것을 알려주는 구문 
- class Meta : 이 폼을 만들기 위해 어떤 model이 쓰여야 하는지 장고에 알려주는 구문 
    - model = post 부분을 통해 

- 이 폼에 필드를 넣으면, title, text만 보여지게 하자 
    - author는 현재 로그인 하고 있는 사람, created_data - 글이 등록되는 시간 

- 뷰애서 이 폼을 사용해 템플릿에서 보여주기만 하자 
    - 링크, URL, 뷰 그리고 템플릿을 만들자

## 폼과 페이지 링크 
- blog/templates/blog/base.html에 링크를 하나 추가
```html
<a href="{% url 'post_new' %}" class="top-menu"><span class="glyphicon glyphicon-plus"></span></a>
```
- post_new라는 새로운 뷰, 부트스트랩 테마에 있는 glyphicon glyphicon-plus 클래스로 더하기 기호가 보이게 됨 
- NoReverseMatch라는 에러가 나옴 ====> ?????

## URL
- blog/urls.py에 구문 추가
```python
path('post/new', views.post_new, name='post_new'),
```
- 브라우저에 사이트를 다시 불러오면 AttributeError
    - 아직 post_new 뷰를 구현하지 않아서 

## post_new view
- blog/views.py 
```python
from .forms import PostForm

// ....

def post_new(request):
    form = PostForm()
    return render(request, 'blog/post_edit.html', { 'form': form})
```
- 새 Post 폼을 추가하기 위해 PostForm() 함수를 호출하도록 해 템플릿에 넘김 

## 템플릿 
- blog/templates/blog 디렉터리 안에 post_edit.html 파일을 생성해 폼이 작동할 수 있게 하자
    ```html
    {% extends 'blog/base.html' %}

    {% block content %}
        <h1>New post</h1>
        <form method="POST" class="post-form">{% csrf_token %}
            {{ form.as_p }}
            <button type="submit" class="save btn btn-default">Save</button>
        </form>
    {% endblock %}
    ```
- 먼저 폼이 보여야 함 : `{{ form.as_p }}`
    - HTML 태그로 폼을 가두어야 함 : `<form method="POST">...</form>`
- Save 버튼이 필요함 => HTML 버튼으로 만들 수 있음 
    - `<button type="submit">Save</button>`
- 마지막으로 `<form ...>`을 열어 `{% csrf_token %}`를 추가해야 함
    - 폼 보안을 위해 중요하다고 함 => 장고는 이렇게 불평할 것

- 이 상태에서 title, text 필드에 아무거나 입력하고 저장해보자 
    - 글이 사라지는 문제 => 입력한 글들이 어디론가 사라지고 새 글이 추가되지 않음 
    - view 추가 적업이 필요할 뿐 

## 폼 저장하기 
- blog/views.py
```python
def post_new(request):
    form = PostForm()
    return render(request, 'blog/post_edit.html', {'form': form})
```
- 폼을 제출할 때 같은 뷰를 불러옴, 이 때 request에는 우리가 입력했던 데이터들을 가지고 있음 
    - request.POST가 이 데이터를 가짐 
    - HTML에서 `<form>`정의에 method="POST"라는 속성이 있던거 기억함?
        - 이렇게 POST로 넘겨진 폼 필드의 value들은 request.POST에 저장됨 
        - POST로 된 value를 다른 거로 바꾸면 안 됨 
    
- view에서 2가지 상황
    1. 첫 번째 : 처음 페이지에 접속했을 때 => 우리가 새 글을 쓸 수 있게 폼이 비어있어야 함
    2. 두 번째 : 폼에 입력된 데이터를 view 페이지로 가지고 올 때 
        - 조건문이 필요함 
    ```python
    # blog/views.py
    if request.method == "POST":
        ...
    else:
        form = PostForm()
    ```
    - 만약 method가 POST라면, 폼에서 받은 데이터를 PostForm으로 넘겨주어야겠지?
    ```python
        form = PostForm(request.POST)
    ```
    - 폼에 들어있는 value들이 올바른지를 확인해야 함 
        - 모든 필드에는 value가 있어야 하고 잘못된 value가 있다면 저장하면 되지 않아야 함
        - `form.is_valid()`를 사용해야 함
        - 폼에 입력된 value가 올바른지 확인한 다음, 저장되는 것
    ```python
    if form.is_valid():
        post = form.save(commit=False)
        post.author = request.user
        post.published_date = timezone.now()
        post.save()
    ```
    - 2가지로 이 단계를 나누어보자
    1. form.save()로 폼을 저장하는 작업 
    2. 작성자를 추가하는 작업 
    - PostForm에는 작성자 필드가 없음 but 필드 value가 필요함 
    - commit=False?? 넘겨진 데이터를 바로 Post 모델에 저장하지는 말라는 뜻 
        - why? 작성자를 추가한 다음 저장해야 하니까
        - 대부분의 경우엔 commit=False를 쓰지 않고 바로 form.save()를 사용해서 저장함 
        - 여기선 작성자 정보를 추가하고 저장해야 하므로 commit=False를 사용하는 것 
        - post.save()는 변경사항(작성자 정보를 포함)을 유지 & 새 블로그 글이 만들어질 것 
    - 새 블로그 글을 작성한 다음 post_detail 페이지로 이동할 수 있다면?
        - 이 모듈을 불러와서 사용해야 함 
    ```python
    from django.shortcuts import redirect
    ```
    ```python
    return redirect('post_detail', pk=post.pk)
    ```
    - post_detail : 우리가 이동해야할 뷰의 이름 
        - post_detail 뷰는 pk 변수가 필요함!
        - pk=post.pk를 사용해 뷰에게 value를 넘겨줄 것 
        - 여기서 post는 새로 생성한 블로그 글 

- 사용자가 로그아웃되는 상황이 발생하기도 함 
    - 브라우저가 닫히거나, DB가 재시작된다든가
    - 만약 로그인 되지 않은 상태에서 새 글을 저장한다면?
    - 사용자가 로그인되어 있지 않아 누구 글을 작성하였는지 알 수 없음 
    - 그래서 글을 저장할 때 오류가 발생 & 로그인 시키도록 관리자 페이지가 나타나게 될 것 

## 폼 수정 
- 폼 수정 버튼 추가
```html
<a class="btn btn-default" href="{% url 'post_edit' pk=post.pk %}"><span class="glyphicon glyphicon-pencil"></span></a>
```
- urls.py에 다음 코드 추가
```python
    path('post/<int:pk>/edit/', views.post_edit, name='post_edit'),
```
- post_edit.html 템플릿을 재사용할 것 
    - view를 만드는 것 
    - blog/views.py
    ```python
    def post_edit(request, pk):
    post = get_object_or_404(Post, pk=pk)
    if request.method == "POST":
        form = PostForm(request.POST, instance=post)
        if form.is_valid():
            post = form.save(commit=False)
            post.author = request.user
            post.published_date = timezone.now()
            post.save()
            return redirect('post_detail', pk=post.pk)
    else:
        form = PostForm(instance=post)
    return render(request, 'blog/post_edit.html', {'form': form})
    ```
    - post_new? 아님 
    1. url로부터 추가로 pk 매개변수를 받아서 처리함 
    2. get_object_or_404(Post, pk=pk)를 호출해 수정하고자 하는 글의 Post 모델 인스턴스를 가져옴 
        - pk로 원하는 글을 찾음 
        - 이렇게 가져온 데이터를 폼을 만들 때(글을 수정할 때 폼에 이전에 입력했던 데이터가 있어야 하겠지?) - 폼을 저장할 때 사용하게 됨 
    - blog/views.py
    ```python
    form = PostForm(request.POST, instance=post)

    form = PostForm(instance=post)
    ```
## 보안 
- 나에게만 보이고 다른 사람에게는 보이지 않으려는 버튼은?
- {% if %} 태그를 추가해 관리자로 로그인한 유저들만 링크가 보일 수 있게 만들 것 
```python
{% if user.is_authenticated %}
    <a href="{% url 'post_new' %}" class="top-menu"><span class="glyphicon glyphicon-plus"></span></a>
{% endif %}
```
- {% if %}는 브라우저에 페이지를 요청하는 사용자가 로그인 하는 경우 링크가 발생됨 
    - 새 게시글을 완전히 보호하진 않음 => 바람직함

- 로그인했기 때문에, 페이지 새로고침을 해도 아무것도 표시되지 않을 것 
