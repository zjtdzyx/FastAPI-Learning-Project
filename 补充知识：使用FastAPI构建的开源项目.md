# 补充知识：使用FastAPI构建的开源项目

## 典型的 FastAPI 项目目录结构

```
fastapi_project/
├── app/
│   ├── __init__.py
│   ├── main.py
│   ├── core/
│   │   ├── __init__.py
│   │   ├── config.py
│   │   └── security.py
│   ├── models/
│   │   ├── __init__.py
│   │   └── user.py
│   ├── schemas/
│   │   ├── __init__.py
│   │   └── user.py
│   ├── crud/
│   │   ├── __init__.py
│   │   └── user.py
│   ├── api/
│   │   ├── __init__.py
│   │   └── api_v1/
│   │       ├── __init__.py
│   │       ├── endpoints/
│   │       │   ├── __init__.py
│   │       │   └── users.py
│   │       └── api.py
│   ├── db/
│   │   ├── __init__.py
│   │   ├── base.py
│   │   └── session.py
├── alembic/
│   ├── versions/
│   └── env.py
├── tests/
│   ├── __init__.py
│   └── test_users.py
├── scripts/
│   └── create_superuser.py
├── .env
├── .gitignore
├── pyproject.toml
├── README.md
```

### 目录结构详解

#### `app/` 目录

这是主应用程序包，包含了应用的核心代码。

- `__init__.py`：使该目录成为一个 Python 包。

- `main.py`：应用程序的入口文件，创建 FastAPI 实例并包含路由。

#### `app/core/` 目录

存放核心功能和配置。

- `config.py`：应用的配置文件，使用 Pydantic 的 `BaseSettings` 来管理配置，例如环境变量、数据库连接等。

  ```python
  from pydantic import BaseSettings

  class Settings(BaseSettings):
      API_V1_STR: str = "/api/v1"
      DATABASE_URL: str = "sqlite:///./test.db"

      class Config:
          case_sensitive = True

  settings = Settings()
  ```

- `security.py`：安全相关的功能，例如密码哈希、鉴权工具等。

#### `app/models/` 目录

存放 ORM 模型，定义数据库的表结构。

- `user.py`：定义用户模型，例如使用 SQLAlchemy 或其他 ORM。

  ```python
  from sqlalchemy import Column, Integer, String
  from app.db.base_class import Base
  
  class User(Base):
      __tablename__ = "users"
  
      id = Column(Integer, primary_key=True, index=True)
      email = Column(String, unique=True, index=True, nullable=False)
      hashed_password = Column(String, nullable=False)
  ```

#### `app/schemas/` 目录

存放 Pydantic 模型，用于数据的序列化和验证。

- `user.py`：定义用户相关的请求和响应模型。

  ```python
  from pydantic import BaseModel
  
  class UserBase(BaseModel):
      email: str
  
  class UserCreate(UserBase):
      password: str
  
  class User(UserBase):
      id: int
  
      class Config:
          orm_mode = True
  ```

#### `app/crud/` 目录

存放数据库的增删改查（CRUD）操作。

- `user.py`：封装用户模型的数据库操作。

  ```python
  from sqlalchemy.orm import Session
  from app.models.user import User
  from app.schemas.user import UserCreate
  
  def get_user(db: Session, user_id: int):
      return db.query(User).filter(User.id == user_id).first()
  
  def create_user(db: Session, user: UserCreate):
      db_user = User(email=user.email, hashed_password=user.password)
      db.add(db_user)
      db.commit()
      db.refresh(db_user)
      return db_user
  ```

#### `app/api/` 目录

存放 API 路由和端点。

- `api_v1/`：API 的第一个版本，方便未来的版本升级。

  - `endpoints/`：存放具体的路由处理函数。

    - `users.py`：用户相关的路由。

      ```python
      from fastapi import APIRouter, Depends, HTTPException
      from sqlalchemy.orm import Session
      from app import crud, models, schemas
      from app.api import deps
      
      router = APIRouter()
      
      @router.post("/users/", response_model=schemas.User)
      def create_user(user_in: schemas.UserCreate, db: Session = Depends(deps.get_db)):
          user = crud.user.create_user(db=db, user=user_in)
          return user
      ```

  - `api.py`：将各个路由模块引入并注册到主路由中。

    ```python
    from fastapi import APIRouter
    from app.api.api_v1.endpoints import users
    
    api_router = APIRouter()
    api_router.include_router(users.router, prefix="/users", tags=["users"])
    ```

#### `app/db/` 目录

与数据库连接和会话管理相关的代码。

- `session.py`：创建数据库会话。

  ```python
  from sqlalchemy import create_engine
  from sqlalchemy.orm import sessionmaker

  from app.core.config import settings

  engine = create_engine(settings.DATABASE_URL, connect_args={"check_same_thread": False})
  SessionLocal = sessionmaker(autocommit=False, autoflush=False, bind=engine)
  ```

- `base.py`：导入所有模型，确保在创建数据库时能够识别到。

  ```python
  from app.db.base_class import Base  # noqa
  from app.models.user import User  # noqa
  ```

#### 其他目录和文件

- `alembic/`：数据库迁移工具 Alembic 的配置目录。

- `tests/`：存放测试代码，使用 pytest 等测试框架。

- `scripts/`：存放脚本，例如创建超级用户。

- `.env`：环境变量文件，存储敏感信息和配置。

- `pyproject.toml`：项目依赖管理文件，使用 Poetry 等工具。

- `README.md`：项目的介绍和文档。

## 示例文件

### `main.py`

```python
from fastapi import FastAPI
from app.api.api_v1.api import api_router
from app.core.config import settings

app = FastAPI(
    title="FastAPI 示例项目",
    description="使用 FastAPI 构建的示例项目。",
    version="1.0.0",
)

app.include_router(api_router, prefix=settings.API_V1_STR)
```

### `app/api/deps.py`

```python
from typing import Generator
from sqlalchemy.orm import Session
from app.db.session import SessionLocal

def get_db() -> Generator:
    try:
        db = SessionLocal()
        yield db
    finally:
        db.close()
```

## 项目组织的好处

- **清晰的结构**：按照功能将代码组织在不同的目录下，易于维护和理解。

- **可扩展性**：当需要添加新功能或模块时，可以轻松找到合适的位置。

- **协作友好**：团队成员可以快速熟悉项目结构，提高协作效率。

- **代码复用**：将通用的功能模块化，可以在不同的地方复用代码。

## 学习和参考资源

为了更深入地了解实际项目的目录结构和最佳实践，您可以参考以下开源项目：

1. **FastAPI + PostgreSQL + Celery 实战项目**

   - GitHub 地址：[https://github.com/tiangolo/full-stack-fastapi-postgresql](https://github.com/tiangolo/full-stack-fastapi-postgresql)
   - 该项目提供了完整的全栈示例，包括用户认证、异步任务和前端框架集成。

2. **FastAPI 真实世界应用示例**

   - GitHub 地址：[https://github.com/fastapi-community/fastapi-realworld-example-app](https://github.com/fastapi-community/fastapi-realworld-example-app)
   - 该项目实现了 [RealWorld](https://github.com/gothinkster/realworld) 应用规范，展示了如何使用 FastAPI 构建实际应用。

## 小结

了解并采用合适的项目目录结构，对于构建稳健且可维护的 FastAPI 应用至关重要。通过遵循上述结构，您可以：

- **提高开发效率**：清晰的目录和模块划分，使得开发和定位代码更加快捷。

- **提升代码质量**：模块化的设计促进了代码的重用和测试。

- **适应项目规模的变化**：无论是小型项目还是大型应用，都可以方便地扩展和维护。

