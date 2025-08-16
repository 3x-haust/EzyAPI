이 README는 한국어 버전입니다.  
[Click here for the English version.](./README.md)

![Contributors](https://img.shields.io/github/contributors/3x-haust/Python_Ezy_API?style=flat)
![Forks](https://img.shields.io/github/forks/3x-haust/Python_Ezy_API?style=social?style=flat)
[![Stars](https://img.shields.io/github/stars/3x-haust/Python_Ezy_API?style=flat&logo=GitHub&color=yellow)](https://github.com/3x-haust/Python_Ezy_API/stargazers)
![License](https://img.shields.io/github/license/3x-haust/Python_Ezy_API?style=flat)
[![PyPI](https://img.shields.io/pypi/v/ezyapi?logo=PyPI?style=flat)](https://pypi.org/project/ezyapi/)

</br>

# 목차
- [목차](#목차)
- [첫 번째 단계](#첫-번째-단계)
    - [배경](#배경)
    - [시작하기](#시작하기)
      - [언어](#언어)
      - [전제조건](#전제조건)
      - [설정하기](#설정하기)
    - [어플리케이션 실행하기](#어플리케이션-실행하기)
- [서비스](#서비스)
    - [서비스란?](#서비스란)
    - [서비스 구조](#서비스-구조)
    - [URL 매핑 규칙](#url-매핑-규칙)
    - [서비스 등록](#서비스-등록)
    - [경로 파라미터 예시](#경로-파라미터-예시)
    - [쿼리 파라미터 예시](#쿼리-파라미터-예시)
    - [데코레이터 예시 (@route)](#데코레이터-예시-route)
- [데이터베이스와 엔티티](#데이터베이스와-엔티티)
    - [데이터베이스 설정](#데이터베이스-설정)
    - [엔티티 정의](#엔티티-정의)
      - [기본 엔티티 (간단한 방법)](#기본-엔티티-간단한-방법)
      - [어노테이션을 사용한 고급 엔티티](#어노테이션을-사용한-고급-엔티티)
      - [컬럼 어노테이션 타입](#컬럼-어노테이션-타입)
      - [컬럼 옵션](#컬럼-옵션)
      - [다양한 Primary Key 예시](#다양한-primary-key-예시)
    - [엔티티 관계](#엔티티-관계)
      - [관계 정의](#관계-정의)
      - [관계 타입](#관계-타입)
      - [관련 데이터 로드](#관련-데이터-로드)
      - [관계 로드 예시](#관계-로드-예시)
- [CLI 개요](#cli-개요)
    - [설치](#설치)
    - [명령어](#명령어)
    - [예제](#예제)
      - [새 프로젝트 생성](#새-프로젝트-생성)
      - [리소스 생성](#리소스-생성)
      - [의존성 설치](#의존성-설치)
      - [스크립트 실행](#스크립트-실행)
      - [테스트 실행](#테스트-실행)
      - [코드 린팅](#코드-린팅)
      - [정보 확인](#정보-확인)
      - [프로젝트 초기화](#프로젝트-초기화)
    - [프로젝트 구조](#프로젝트-구조)

</br>
</br>
</br>

# 첫 번째 단계

### 배경

저희는 [Nest.js](https://nestjs.com/)를 좋아하지만, [Nest.js](https://nestjs.com/)의 Controller, Module 등이 간단한 작업에는 불필요하다고 생각했습니다.

### 시작하기

이 문서에서는 Ezy API의 **핵심 원리** 에 대해 알아봅니다. Ezy API 애플리케이션의 필수 구성 요소를 숙지하려면,
기초적인 수준에서 많은 분야를 다루며 기본적인 CRUD 애플리케이션을 구축해 봐야 합니다.

#### 언어

Ezy API는 [Python](https://www.python.org/)언어를 사용합니다.

추후에는 [TypeScript](https://www.typescriptlang.org/), [Java](https://java.com/) 등과 같은 언어에서 사용할 수 있도록 지원할 예정입니다.

#### 전제조건

운영 체제에 [Python](https://www.python.org/)(>= 3.11)가 설치되어 있는지 확인하세요.

#### 설정하기

[Ezy API CLI](#cli-개요) 로 새 프로젝트를 설정하는 것은 매우 간단합니다. [pip](https://pypi.org/project/pip/)이 설치되어 있다면, OS 터미널에서 다음 명령을 사용해 새 Ezy API 프로젝트를 생성할 수 있습니다.


```bash
$ pip install ezyapi
$ ezy new project-name
```

`project-name` 디렉토리가 생성되고, main.py 와 cli 설정 파일들이 생성됩니다.

프로젝트의 기본적인 구조는 아래와 같습니다.
```
app_service.py
ezy.json
main.py
```
> **팁**
> 
> 위의 파일은 [이곳에서](https://github.com/3x-haust/Python_Ezy_API/tree/main/example) 확인할 수 있습니다.

<br></br>

위 핵심 파일들을 간단하게 설명하면 아래와 같습니다.

|파일명|설명|
|:---:|:---|
|`app_service.py`|기본적인 서비스 파일|
|`ezy.json`|cli 명령어 설정 파일|
|`main.py`|시작 파일(entry file). 핵심 함수인 `EzyAPI`를 사용하여 Ezy API 어플리케이션 인스턴스를 만듭니다.|

> 위에서 서술한 서비스 등은 지금은 이해 못하셔도 괜찮습니다. 이후에 나올 챕터들에서 자세한 설명이 나옵니다!

<br><br/>

간단하게 main.py 파일부터 만들어 보겠습니다. 해당 파일에는 어플리케이션을 시작해주는 main 모듈이 있습니다.

```python
# main.py
from ezyapi import EzyAPI
from ezyapi.database import DatabaseConfig
from user.user_service import UserService
from app_service import AppService

app = EzyAPI()

if __name__ == "__main__":
    app.run(port=8000)
```

> **팁**
>>
> 개발 중에는 코드 변경 시 자동으로 서버를 재시작하려면 `reload=True` 옵션을 사용할 수 있습니다.

### 어플리케이션 실행하기

OS 터미널에서 다음 명령을 어플리케이션을 실행할 수 있습니다.
```bash
$ ezy run start
```

</br>
</br>
</br>

# 서비스

### 서비스란?

Ezy API에서 **서비스(Service)** 는 요청을 처리하고 비즈니스 로직을 수행하는 핵심 구성 요소입니다.  
[Nest.js](https://nestjs.com)의 Controller나 Service와 비슷한 역할을 하지만, Ezy API는 더 간결하고 직관적인 방식으로 서비스만으로도 충분히 API를 구성할 수 있도록 설계되었습니다.

### 서비스 구조

서비스는 `EzyService` 클래스를 상속받아 생성됩니다.  
아래는 기본적인 서비스의 예시입니다.
> **팁**
>
> 서비스는 ```$ ezy g res user``` 사용해 생성 할 수 있습니다

```python
# app_service.py
from ezyapi import EzyService

class AppService(EzyService):
    async def get_app(self) -> str:
        return "Hello, World!"
```

- `EzyService`를 상속하면, 서비스 내에서 비동기 함수로 API 엔드포인트를 정의할 수 있습니다.
- 함수명은 곧 API의 엔드포인트 URL이 됩니다.
  - 예를 들어 `get_user`이라는 함수는 `/user/` 경로에 `GET`메소드로 자동 매핑됩니다.
    - 단 예외적으로 서비스 명이 `app`인 경우에는 루트 경로로 매핑 됩니다.
- 함수는 `async`로 정의하여 비동기 처리가 가능합니다.

### URL 매핑 규칙

서비스의 함수 이름이 URL 엔드포인트로 자동으로 매핑됩니다.

| 함수명 | HTTP 메서드 | URL |
|:---:|:---:|:---|
|`get_user`|GET|`/user/`|
|`list_users`|GET|`/user/`|
|`create_user`|POST|`/user/`|
|`update_user`|PUT|`/user/`|
|`delete_user`|DELETE|`/user/`|
|`edit_user`|PATCH|`/user/`|

> **팁**
>
> `get`, `update`, `delete`, `edit` 메소드들은 `by_id` 등을 이용하여 경로 파라미터를 이용할 수 있습니다.  
> 예: `get_user_by_id` ➡️ `GET /user/{id}`

### 서비스 등록

서비스는 `main.py`에서 EzyAPI 인스턴스에 등록할 수 있습니다.

```python
# main.py
from ezyapi import EzyAPI
from ezyapi.database import DatabaseConfig
from app_service import AppService

app = EzyAPI()
app.add_service(AppService)

if __name__ == "__main__":
    app.run(port=8000)
```

### 경로 파라미터 예시

Ezy API는 함수명에 `by_id`, `by_name` 등을 붙이면 URL 경로에 파라미터를 자동으로 매핑합니다.

```python
# user_service.py
from ezyapi import EzyService

class UserService(EzyService):
    async def get_user_by_id(self, id: int) -> dict:
        return {"id": id, "name": "John Doe"}
```

- `get_user_by_id` ➡️ `GET /user/{id}` 경로로 자동 매핑됩니다.
- `id`는 URL 경로에서 `path parameter`로 사용됩니다.

**요청 예시**

```http
GET /user/10
```

**응답 예시**

```json
{
  "id": 10,
  "name": "John Doe"
}
```

### 쿼리 파라미터 예시

쿼리 파라미터는 함수 인자에서 `Optional`과 기본값을 정의하면, 쿼리 스트링으로 받을 수 있습니다.

```python
# user_service.py
from ezyapi import EzyService
from typing import Optional, List

class UserService(EzyService):
    async def list_users(self, name: Optional[str] = None, age: Optional[int] = None) -> List[dict]:
        filters = {}
        if name:
            filters["name"] = name
        if age:
            filters["age"] = age

        return [{"id": 1, "name": name or "John", "age": age or 25}]
```

- `list_users` ➡️ `GET /user/`
- 쿼리스트링으로 `name`, `age`를 전달할 수 있습니다.

**요청 예시**

```http
GET /user/?name=Alice&age=30
```

**응답 예시**

```json
[
  {
    "id": 1,
    "name": "Alice",
    "age": 30
  }
]
```

### 데코레이터 예시 (@route)

서비스 함수에 직접 `@route()` 데코레이터를 사용하면 URL과 메서드를 수동으로 지정할 수 있습니다.

```python
# user_service.py
from ezyapi import EzyService, route

class UserService(EzyService):
    @route('get', '/name/{name}', description="Get user by name")
    async def get_user_by_name(self, name: str) -> dict:
        return {"name": name, "email": "example@example.com"}
```

- `@route('get', '/name/{name}')` ➡️ `GET /name/{name}` 경로로 설정됩니다.
- `description`은 API 문서화에 사용됩니다.

**요청 예시**

```http
GET /name/Alice
```

**응답 예시**

```json
{
  "name": "Alice",
  "email": "example@example.com"
}
```

> **팁**
>
> `@route()` 데코레이터를 사용하면 자동 매핑을 오버라이드하여 원하는 URL, HTTP 메서드를 자유롭게 설정할 수 있습니다.

</br>
</br>
</br>

# 데이터베이스와 엔티티

### 데이터베이스 설정

Ezy API는 SQLite, MySQL, PostgreSQL, MongoDB 등 다양한 데이터베이스 타입을 지원합니다.

```python
# main.py
from ezyapi import EzyAPI
from ezyapi.database import DatabaseConfig
from user.user_service import UserService

if __name__ == "__main__":
    app = EzyAPI()
    
    # SQLite 설정
    db_config = DatabaseConfig(
        db_type="sqlite",
        connection_params={
            "dbname": "./app.db"
        }
    )
    
    # 또는 MySQL 설정
    # db_config = DatabaseConfig(
    #     db_type="mysql",
    #     connection_params={
    #         "host": "localhost",
    #         "port": 3306,
    #         "user": "root",
    #         "password": "password",
    #         "dbname": "myapp"
    #     }
    # )
    
    app.configure_database(db_config)
    app.add_service(UserService)
    app.run(port=8000)
```

### 엔티티 정의

엔티티는 `EzyEntityBase`를 상속받아 정의합니다. 고급 컬럼 설정을 위해 TypeORM 스타일의 어노테이션을 사용할 수 있습니다.

#### 기본 엔티티 (간단한 방법)

```python
# user/entity/user_entity.py
from ezyapi import EzyEntityBase

class UserEntity(EzyEntityBase):
    def __init__(self, id: int = None, name: str = "", email: str = ""):
        self.id = id
        self.name = name
        self.email = email

# 이렇게 하면 자동으로 id가 PrimaryGeneratedColumn이 됩니다
```

#### 어노테이션을 사용한 고급 엔티티

데이터베이스 컬럼에 대한 더 세밀한 제어를 위해 어노테이션을 사용할 수 있습니다:

```python
# user/entity/user_entity.py
from typing import Annotated
from ezyapi import EzyEntityBase, PrimaryGeneratedColumn, Column

class UserEntity(EzyEntityBase):
    def __init__(self, email: str = ""):
        self.email = email
    
    # 어노테이션을 사용하여 명시적으로 PrimaryGeneratedColumn 지정
    id: Annotated[int, PrimaryGeneratedColumn()] = None
    
    # 특별한 설정이 필요한 필드에만 어노테이션 추가
    name: Annotated[str, Column(nullable=False, column_type="VARCHAR(100)")] = ""
```

#### 컬럼 어노테이션 타입

| 어노테이션 | 설명 | 예시 |
|:---|:---|:---|
| `PrimaryColumn()` | 커스텀 primary key (사용자 지정 값) | `id: Annotated[str, PrimaryColumn(column_type="VARCHAR(50)")] = None` |
| `PrimaryGeneratedColumn()` | 자동 증가 primary key | `id: Annotated[int, PrimaryGeneratedColumn(column_type="BIGINT")] = None` |
| `Column()` | 옵션이 있는 일반 컬럼 | `name: Annotated[str, Column(nullable=False, unique=True)] = ""` |

#### 컬럼 옵션

- `column_type`: 데이터베이스 컬럼 타입 지정 (예: "VARCHAR(100)", "TEXT", "BIGINT")
- `nullable`: 컬럼이 NULL을 허용할지 여부 (기본값: True)
- `unique`: 컬럼에 unique 제약조건을 적용할지 여부 (기본값: False)
- `auto_increment`: 컬럼이 자동 증가할지 여부 (Column은 기본값 False, PrimaryGeneratedColumn은 True)

#### 다양한 Primary Key 예시

```python
# 기본 자동 증가 primary key (명시적 어노테이션)
class TodoEntity(EzyEntityBase):
    def __init__(self, content: str = "", completed: bool = False):
        self.content = content
        self.completed = completed

    id: Annotated[int, PrimaryGeneratedColumn()] = None

# 문자열 primary key
class ProductEntity(EzyEntityBase):
    def __init__(self, name: str = "", price: float = 0.0):
        self.name = name
        self.price = price
        
    product_code: Annotated[str, PrimaryColumn(column_type="VARCHAR(20)")] = None

# 커스텀 자동 증가 primary key
class OrderEntity(EzyEntityBase):
    def __init__(self, user_id: int = 0, total_amount: float = 0.0):
        self.user_id = user_id
        self.total_amount = total_amount
        
    order_id: Annotated[int, PrimaryGeneratedColumn(column_type="BIGINT")] = None
```

> **참고**
>
> - 어노테이션은 **선택사항**입니다 - 특별한 데이터베이스 설정이 필요한 필드에만 추가하면 됩니다
> - 어노테이션이 없는 필드는 기본 동작을 사용합니다 (일반 컬럼)
> - `__init__` 메서드에 `id` 파라미터가 있으면 자동으로 자동 증가 primary key가 됩니다

### 엔티티 관계

Ezy API는 TypeORM 스타일의 엔티티 관계를 지원하며, OneToMany와 ManyToOne 관계를 정의할 수 있습니다. 엔티티 간의 관계를 정의하고 연관된 데이터를 효율적으로 로드할 수 있습니다.

#### 관계 정의

```python
# user/entity/user_entity.py
from typing import List
from ezyapi import EzyEntityBase, OneToMany, ManyToOne

class UserEntity(EzyEntityBase):
    def __init__(self, id: int = None, name: str = "", email: str = ""):
        self.id = id
        self.name = name
        self.email = email
    
    # OneToMany 관계: 한 사용자가 여러 게시글을 가질 수 있음
    posts: List['PostEntity'] = OneToMany('PostEntity', 'user_id')

class PostEntity(EzyEntityBase):
    def __init__(self, id: int = None, title: str = "", content: str = "", user_id: int = None):
        self.id = id
        self.title = title
        self.content = content
        self.user_id = user_id
    
    # ManyToOne 관계: 여러 게시글이 한 사용자에게 속할 수 있음
    user: 'UserEntity' = ManyToOne('UserEntity', 'user_id')
```

#### 관계 타입

| 관계 | 설명 | 예시 |
|:---|:---|:---|
| `OneToMany(target_entity, mapped_by)` | 하나의 엔티티가 여러 관련 엔티티를 가짐 | `posts: List['PostEntity'] = OneToMany('PostEntity', 'user_id')` |
| `ManyToOne(target_entity, foreign_key)` | 여러 엔티티가 하나의 엔티티에 속함 | `user: UserEntity = ManyToOne(UserEntity, 'user_id')` |

#### 관련 데이터 로드

저장소 메서드에서 `relations` 파라미터를 사용하여 관련 엔티티를 로드할 수 있습니다:

```python
# user_service.py
from ezyapi import EzyService
from ezyapi.database import DatabaseConfig
from user.entity.user_entity import UserEntity

class UserService(EzyService):
    def __init__(self):
        db_config = DatabaseConfig.get_instance()
        self.user_repository = db_config.get_repository(UserEntity)
    
    async def get_users_with_posts(self):
        # 사용자와 함께 게시글 로드
        users = await self.user_repository.find(relations=["posts"])
        return users
    
    async def get_user_with_posts_by_id(self, user_id: int):
        # 특정 사용자와 함께 게시글 로드
        user = await self.user_repository.find_one(
            where={"id": user_id}, 
            relations=["posts"]
        )
        return user

# post_service.py
from post.entity.post_entity import PostEntity

class PostService(EzyService):
    def __init__(self):
        db_config = DatabaseConfig.get_instance()
        self.post_repository = db_config.get_repository(PostEntity)
    
    async def get_posts_with_users(self):
        # 게시글과 연관된 사용자 로드
        posts = await self.post_repository.find(relations=["user"])
        return posts
```

#### 관계 로드 예시

```python
# 여러 관계 로드
users = await user_repository.find(relations=["posts", "profile", "comments"])

# 관계와 함께 특정 사용자 로드
user = await user_repository.find_one(
    where={"id": 1}, 
    relations=["posts"]
)

# 로드된 사용자 객체에는 posts 속성이 채워져 있음
print(user.posts)  # PostEntity 객체 목록
```

</br>
</br>
</br>

# CLI 개요

Ezy CLI는 Ezy API 프로젝트 관리를 간소화하기 위한 명령줄 인터페이스 도구입니다. 프로젝트 생성, 빌드, 테스트, 실행 등을 쉽게 수행할 수 있는 다양한 명령어를 제공합니다.

### 설치

Ezy CLI는 pip를 사용하여 설치할 수 있습니다:

```bash
$ pip install ezyapi
```

> **참고**
>
> 시스템에 Python 3.11 이상 버전이 설치되어 있는지 확인하세요.

### 명령어

Ezy CLI는 다음 명령어를 지원합니다:

| 명령어 | 설명 |
|:---|:---|
|`new <project_name>`|지정된 이름으로 새로운 Ezy API 프로젝트를 생성합니다.|
|`generate <type> <name>` 또는 `g <type> <name>`|지정된 유형(예: 'res' for resource)과 이름으로 컴포넌트를 생성합니다.|
|`install [packages...]` 또는 `install -r <requirements.txt>`|ezy_modules 디렉토리에 의존성을 설치합니다. 패키지를 직접 지정하거나 requirements.txt 파일을 사용할 수 있습니다.|
|`run <script>`|ezy.json에 정의된 스크립트를 실행합니다 (예: 'dev' 또는 'start').|
|`test`|'test' 디렉토리에 있는 테스트를 pytest로 실행합니다.|
|`lint`|flake8을 사용하여 코드 스타일을 점검합니다.|
|`info`|CLI 버전, Python 버전, 플랫폼, 현재 디렉토리 정보를 표시합니다.|
|`init [project_name]`|현재 디렉토리에 새로운 Ezy 프로젝트를 초기화합니다. 프로젝트 이름을 지정하지 않으면 현재 디렉토리 이름이 사용됩니다.|

### 예제

#### 새 프로젝트 생성

```bash
$ ezy new my_project
```

`my_project`라는 이름의 디렉토리에 Ezy API 프로젝트의 기본 구조를 생성합니다.

#### 리소스 생성

```bash
$ ezy generate res user
```

"user"라는 이름의 리소스를 생성하며, 선택적으로 CRUD 작업을 포함합니다.

#### 의존성 설치

ezy.json에 나열된 의존성을 설치하려면:

```bash
$ ezy install
```

특정 패키지를 설치하려면:

```bash
$ ezy install requests numpy
```

requirements.txt 파일에서 설치하려면:

```bash
$ ezy install -r requirements.txt
```

#### 스크립트 실행

ezy.json에 정의된 스크립트가 있다고 가정할 때, 예를 들어:

```json
{
  "scripts": {
    "start": "python3 main.py",
    "dev": "python3 main.py --dev"
  }
}
```

다음과 같이 실행할 수 있습니다:

```bash
$ ezy run start
```

#### 테스트 실행

테스트를 실행하려면:

```bash
$ ezy test
```

> **참고**
>
> pytest가 설치되어 있어야 합니다.

#### 코드 린팅

코드 스타일 문제를 점검하려면:

```bash
$ ezy lint
```

> **참고**
>
> flake8이 설치되어 있어야 합니다.

#### 정보 확인

CLI 및 시스템 정보를 표시하려면:

```bash
$ ezy info
```

#### 프로젝트 초기화

현재 디렉토리에 새 Ezy 프로젝트를 초기화하려면:

```bash
$ ezy init
```

### 프로젝트 구조

`ezy new <project_name>` 명령으로 새 프로젝트를 생성하면 다음 구조가 생성됩니다:

```text
project_name/
├── ezy.json
├── main.py
├── app_service.py
├── test/
│   └── (테스트 파일)
├── ezy_modules/
│   └── (설치된 의존성)
└── .gitignore
```

- `ezy.json`: 프로젝트 설정(의존성 및 스크립트 포함).
- `main.py`: 애플리케이션 진입점.
- `app_service.py`: 예제 서비스.
- `test/`: 테스트 파일 디렉토리.
- `ezy_modules/`: 프로젝트별 의존성을 위한 디렉토리.
- `.gitignore`: Git 무시 파일.

`ezy generate res <name>` 명령으로 리소스를 생성하면 다음 구조가 생성됩니다:

```text
<name>/
├── __init__.py
├── dto/
│   ├── __init__.py
│   ├── <name>_create_dto.py
│   └── <name>_update_dto.py
├── entity/
│   ├── __init__.py
│   └── <name>_entity.py
└── <name>_service.py
```

또한 `test/test_<name>_service.py`에 테스트 파일이 생성됩니다.

> **참고**
>
> - CLI는 ANSI 색상을 지원하는 터미널에서 가독성을 높이기 위해 색상 코드를 사용합니다.
> - `test` 명령은 pytest가 설치되어 있어야 합니다. 새 프로젝트의 기본 의존성에 포함되어 있습니다.
> - `lint` 명령은 flake8이 필요합니다. 별도로 설치해야 할 수 있습니다.
> - `update` 명령은 현재 실제 업데이트를 수행하지 않고 시뮬레이션만 합니다.

