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


