
# FastAPI Learning Project

![FastAPI](https://img.shields.io/badge/FastAPI-⭐%204.8.0-blue)

## 概述

欢迎来到 **FastAPI 学习项目**！这个项目旨在系统地学习和实践 FastAPI 的核心功能，包括构建 API、处理表单数据与文件上传、实现用户认证与授权、与数据库集成、部署应用以及使用 WebSockets 构建实时应用。

通过这个项目，您将掌握构建高性能、可扩展且安全的 Web 应用程序所需的技能，并且可以参考开源项目来进一步提升自己的能力。

## 目录结构

```
fastapi-learning-project/
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
│   │       │   ├── users.py
│   │       │   └── websocket.py
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
├── static/
│   └── chat.html
├── .env
├── .gitignore
├── pyproject.toml
├── README.md
```

## 功能简介

1. **API 构建**
   - 创建高性能的 API 端点。
   - 处理表单数据与文件上传。

2. **用户认证与授权**
   - 实现基于 OAuth2 和 JWT 的用户认证。
   - 权限控制，确保安全访问。

3. **数据库集成**
   - 使用 SQLAlchemy 进行 ORM 操作。
   - 实现数据的增删改查（CRUD）。

4. **WebSockets**
   - 构建实时聊天应用，支持多客户端通信。

5. **部署**
   - 使用 Uvicorn 和 Gunicorn 部署应用。
   - Docker 容器化部署。
   - 配置 Nginx 作为反向代理。

## 安装与设置

### 前提条件

- **Python 3.9+**
- **Poetry**（用于依赖管理）

### 步骤

1. **克隆仓库**

   ```bash
   git clone https://github.com/yourusername/fastapi-learning-project.git
   cd fastapi-learning-project
   ```

2. **安装依赖**

   使用 Poetry 安装项目依赖：

   ```bash
   poetry install
   ```

3. **配置环境变量**

   创建 `.env` 文件并添加必要的配置：

   ```env
   DATABASE_URL=sqlite:///./sql_app.db
   SECRET_KEY=your-secret-key
   ALGORITHM=HS256
   ACCESS_TOKEN_EXPIRE_MINUTES=30
   ```

4. **初始化数据库**

   ```bash
   poetry run alembic upgrade head
   ```

## 运行项目

### 使用 Uvicorn

启动 FastAPI 应用：

```bash
poetry run uvicorn app.main:app --reload
```

访问 API 文档： [http://localhost:8000/docs](http://localhost:8000/docs)

### 使用 Gunicorn

```bash
poetry run gunicorn -w 4 -k uvicorn.workers.UvicornWorker app.main:app
```

## 使用 WebSockets

1. **启动应用**

   确保应用正在运行。

2. **打开前端页面**

   打开 `static/chat.html` 文件，在浏览器中使用多个标签页进行测试。

3. **实时聊天**

   在不同的标签页中输入用户名并发送消息，实时查看通讯效果。

## 项目结构详解

### `app/` 目录

主应用程序包，包含核心代码。

- **`main.py`**：应用入口，初始化 FastAPI 实例并包含路由。
- **`core/`**：配置与安全相关模块。
- **`models/`**：ORM 模型，定义数据库表结构。
- **`schemas/`**：Pydantic 模型，用于数据验证与序列化。
- **`crud/`**：封装数据库操作。
- **`api/`**：API 路由与端点，包括 HTTP 路由和 WebSocket 路由。
- **`db/`**：数据库连接与会话管理。

### `alembic/` 目录

数据库迁移工具 Alembic 的配置目录。

### `tests/` 目录

测试代码，使用 pytest 进行测试。

### `scripts/` 目录

实用脚本，如创建超级用户。

### `static/` 目录

静态文件，如前端页面。

## 贡献

非常欢迎您的贡献！请遵循以下步骤：

1. Fork 本仓库
2. 创建您的分支 (`git checkout -b feature/AmazingFeature`)
3. 提交您的更改 (`git commit -m 'Add some AmazingFeature'`)
4. 推送到分支 (`git push origin feature/AmazingFeature`)
5. 打开 Pull Request

## 许可证

本项目采用 [MIT 许可证](LICENSE) 。

## 参考资料

- **FastAPI 官方文档**：[https://fastapi.tiangolo.com/](https://fastapi.tiangolo.com/)
- **SQLAlchemy 文档**：[https://www.sqlalchemy.org/](https://www.sqlalchemy.org/)
- **Alembic 文档**：[https://alembic.sqlalchemy.org/](https://alembic.sqlalchemy.org/)
- **WebSockets 规范**：[https://tools.ietf.org/html/rfc6455](https://tools.ietf.org/html/rfc6455)

## 致谢

感谢所有支持和贡献本项目的开发者和社区成员！

---

希望这个项目能帮助你更好地学习和掌握 FastAPI。如果有任何问题或建议，欢迎提出！

