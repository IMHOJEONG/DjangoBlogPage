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