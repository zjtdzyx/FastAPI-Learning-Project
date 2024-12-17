# 第一阶段——初识Poetry和FastAPI

## 目录

1. [什么是 Poetry 和 FastAPI？](#什么是-poetry-和-fastapi)
2. [为什么选择 Poetry 和 FastAPI？](#为什么选择-poetry-和-fastapi)
3. [创建项目的基本流程](#创建项目的基本流程)
    - [安装 Poetry](#安装-poetry)
    - [创建新的项目](#创建新的项目)
    - [配置项目依赖](#配置项目依赖)
    - [编写 FastAPI 应用](#编写-fastapi-应用)
    - [运行开发服务器](#运行开发服务器)
4. [补充知识](#补充知识)
    - [Poetry 的优势](#poetry-的优势)
    - [FastAPI 的特点](#fastapi-的特点)
    - [虚拟环境与依赖管理](#虚拟环境与依赖管理)
    - [部署 FastAPI 应用](#部署-fastapi-应用)
5. [总结](#总结)

---

## 什么是 Poetry 和 FastAPI？

- **Poetry**：是一个用于管理 Python 项目的依赖、打包和发布的工具。它简化了依赖管理流程，提供了统一的项目配置文件 (`pyproject.toml`)。

- **FastAPI**：是一个现代、快速（高性能）的 Web 框架，用于基于标准 Python 类型提示构建 API。它基于 Starlette 和 Pydantic，提供自动生成的文档、数据验证等功能。

---

## 为什么选择 Poetry 和 FastAPI？

- **Poetry**：
    - **简化依赖管理**：自动处理依赖解析和冲突。
    - **虚拟环境集成**：自动创建和管理虚拟环境，隔离项目依赖。
    - **统一配置**：使用 `pyproject.toml` 文件统一管理项目配置。
    - **打包与发布**：简化 Python 包的构建和发布流程。

- **FastAPI**：
    - **高性能**：基于 ASGI 和 Starlette，性能接近 Node.js 和 Go。
    - **自动生成文档**：集成 Swagger UI 和 ReDoc，自动生成交互式 API 文档。
    - **类型提示支持**：利用 Python 类型提示，实现数据验证和编辑器支持。
    - **易于使用**：简洁且直观的 API 设计，快速开发。

---

## 创建项目的基本流程

下面，我们将一步步完成使用 Poetry 和 FastAPI 创建一个简单项目的过程。

### 1. 安装 Poetry

首先，确保你的系统已经安装了 **Poetry**。你可以使用以下命令安装 Poetry：

```sh
curl -sSL https://install.python-poetry.org | python3 -
```

安装完成后，确保将 Poetry 添加到你的系统路径中。你可以通过以下命令验证安装是否成功：

```sh
poetry --version
```

输出示例：

```
Poetry version 1.4.0
```

### 2. 创建新的项目

使用 Poetry 创建一个新的项目。例如，创建一个名为 `fastapi-project` 的项目：

```sh
poetry new fastapi-project
```

该命令将生成以下结构：

```
fastapi-project/
├── README.rst
├── pyproject.toml
├── fastapi_project
│   └── __init__.py
└── tests
    └── __init__.py
```

进入项目目录：

```sh
cd fastapi-project
```

### 3. 配置项目依赖

使用 Poetry 添加 **FastAPI** 和 **Uvicorn** 作为运行依赖：

```sh
poetry add fastapi uvicorn
```

- **FastAPI**：用于构建 API。
- **Uvicorn**：ASGI 服务器，用于运行 FastAPI 应用。

如果需要添加开发依赖（例如测试框架），可以使用 `--dev` 选项。例如，添加 `pytest`：

```sh
poetry add pytest --dev
```

### 4. 编写 FastAPI 应用

在项目目录中，进入主文件夹并创建一个 `main.py` 文件：

```sh
cd fastapi_project
touch main.py
```

编辑 `main.py`，添加以下代码：

```python
# main.py

from fastapi import FastAPI

app = FastAPI()

@app.get("/")
def read_root():
    return {"Hello": "World"}

@app.get("/items/{item_id}")
def read_item(item_id: int, q: str = None):
    return {"item_id": item_id, "q": q}
```

**解释**：

- **FastAPI 实例**：`app = FastAPI()`
- **根路由**：访问 `/` 返回 `{"Hello": "World"}`。
- **动态路由**：访问 `/items/{item_id}` 返回包含 `item_id` 和可选查询参数 `q` 的 JSON 响应。

### 5. 运行开发服务器

返回项目根目录：

```sh
cd ..
```

使用 Poetry 运行 Uvicorn 服务器：

```sh
poetry run uvicorn fastapi_project.main:app --reload
```

- `fastapi_project.main:app`：指定应用的位置（模块路径）。
- `--reload`：启用自动重新加载，适用于开发环境。

你应该会看到类似以下的输出：

```
INFO:     Uvicorn running on http://127.0.0.1:8000 (Press CTRL+C to quit)
INFO:     Uvicorn reload process started
```

### 6. 访问应用

打开浏览器，访问 `http://127.0.0.1:8000`，你将看到：

```json
{
  "Hello": "World"
}
```

访问 `http://127.0.0.1:8000/items/1?q=test`，你将看到：

```json
{
  "item_id": 1,
  "q": "test"
}
```

### 7. 查看自动生成的文档

FastAPI 自动生成交互式 API 文档：

- **Swagger UI**：`http://127.0.0.1:8000/docs`
- **ReDoc**：`http://127.0.0.1:8000/redoc`

这些文档允许你直接在浏览器中测试 API 端点，非常适合开发和调试。

### 8. 进入 Poetry 的虚拟环境（可选）

Poetry 自动为项目创建虚拟环境。你可以通过以下命令激活虚拟环境：

```sh
poetry shell
```

在虚拟环境中，你可以直接运行 Python 命令或脚本，而无需每次都使用 `poetry run`。

退出虚拟环境：

```sh
exit
```

### 9. 其他常用 Poetry 命令

- **查看项目依赖**：

    ```sh
    poetry show
    ```

- **更新依赖**：

    ```sh
    poetry update
    ```

- **移除依赖**：

    ```sh
    poetry remove <package_name>
    ```

- **安装所有依赖（适用于克隆的项目）**：

    ```sh
    poetry install
    ```

---

## 补充知识

### Poetry 的优势

- **依赖解析**：自动解决依赖冲突，确保依赖版本的兼容性。
- **锁定文件**：生成 `poetry.lock` 文件，确保在不同环境中安装相同版本的依赖。
- **简化发布流程**：使用简单的命令打包和发布 Python 包到 PyPI。
- **兼容性**：支持 Poetry 与其他工具（如 GitHub Actions）的集成。

### FastAPI 的特点

- **高性能**：基于 Starlette 和 Pydantic，性能优异，适合生产环境。
- **自动文档**：无需额外配置，即可生成 Swagger UI 和 ReDoc 文档。
- **类型提示**：利用 Python 类型提示，实现数据验证和编辑器支持，提高代码质量。
- **异步支持**：原生支持异步编程（`async`/`await`），适用于高并发场景。
- **依赖注入**：内置依赖注入系统，简化复杂应用的结构。

### 虚拟环境与依赖管理

- **虚拟环境**：隔离项目的 Python 环境，避免不同项目之间的依赖冲突。
- **Poetry 管理虚拟环境**：简化虚拟环境的创建和管理，用户无需手动使用 `venv` 或 `virtualenv`。

### 部署 FastAPI 应用

在开发完成后，你可能需要将应用部署到生产环境。常用的方法包括：

- **使用 Uvicorn 启动**：

    ```sh
    uvicorn fastapi_project.main:app --host 0.0.0.0 --port 80
    ```

- **结合 Gunicorn**：

    使用 Gunicorn 作为进程管理器，Uvicorn 作为工作进程。

    ```sh
    gunicorn fastapi_project.main:app -w 4 -k uvicorn.workers.UvicornWorker
    ```

- **Docker 部署**：

    创建 `Dockerfile`，将应用容器化，方便部署到各种云平台。

    示例 `Dockerfile`：

    ```dockerfile
    FROM python:3.11-slim

    WORKDIR /app

    COPY pyproject.toml poetry.lock ./
    RUN pip install poetry && poetry install --no-dev

    COPY . .

    CMD ["poetry", "run", "uvicorn", "fastapi_project.main:app", "--host", "0.0.0.0", "--port", "80"]
    ```

- **云平台**：

    - **Heroku**：通过 Git 部署。
    - **AWS**：使用 EC2、Elastic Beanstalk 或 Lambda（与 API Gateway 结合）。
    - **DigitalOcean**、**Vercel** 等其他云服务。

---

## 总结

通过以上步骤和知识点，你应该已经掌握了如何使用 **Poetry** 和 **FastAPI** 创建和管理一个基础的项目。从安装和配置 Poetry，到编写和运行 FastAPI 应用，再到理解它们各自的优势和应用场景。这些工具结合起来，可以显著提升你的开发效率和代码质量。

**建议**：

- **深入学习 FastAPI**：探索更多高级功能，如中间件、依赖注入、数据库集成等。
- **掌握 Poetry**：了解更多高级用法，如自定义脚本、插件等。
- **实践项目**：通过实际项目练习，加深对工具的理解和应用。

