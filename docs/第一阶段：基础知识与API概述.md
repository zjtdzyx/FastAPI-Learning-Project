# 第一阶段：基础知识与API概述

## 学习计划概览

我们将把学习分为以下几个部分，每个部分都会包含简短的理论介绍和实战练习：

1. **快速回顾 FastAPI 基础**
2. **构建你的第一个 API 端点**
3. **路径参数和查询参数的使用**
4. **请求体和数据模型（Pydantic）**
5. **表单数据和文件上传**
6. **处理错误和响应**
7. **中间件与依赖注入**
8. **认证与授权**
9. **与数据库的集成**
10. **部署你的 FastAPI 应用**

接下来，让我们一步一步开始学习

---

### **第一步：快速回顾 FastAPI 基础**

#### **目标**

- 了解 FastAPI 的核心特点和优势
- 理解 ASGI，异步编程的基础

#### **内容介绍**

**FastAPI** 是一个用于构建 **API** 的现代、高性能 Web 框架，具有以下特点：

- **高性能**：基于 **Starlette** 和 **Pydantic**，性能媲美 NodeJS 和 Go。
- **易于使用**：语法简洁，基于 Python 的类型提示，自动完成效率高。
- **自动文档**：内置自动生成交互式文档（Swagger UI 和 ReDoc）。

#### **练习**

1. **创建一个基本的 FastAPI 项目**

   - **创建项目目录**：

     ```bash
     mkdir fastapi_tutorial
     cd fastapi_tutorial
     ```

   - **初始化 Poetry 项目**：

     ```bash
     poetry init -n
     ```

     这将创建一个新的 `pyproject.toml` 文件。

   - **添加 FastAPI 和 Uvicorn 依赖**：

     ```bash
     poetry add fastapi uvicorn[standard]
     ```

   - **创建主应用文件**

     在项目目录下创建一个 `main.py` 文件，内容如下：

     ```python
     from fastapi import FastAPI
  
     app = FastAPI()
  
     @app.get("/")
     async def read_root():
         return {"message": "Hello, FastAPI!"}
     ```

2. **运行应用**

   - **使用 Poetry 启动 Uvicorn 服务器**：

     ```bash
     poetry run uvicorn main:app --reload
     ```

   - **测试应用**

     在浏览器中访问 `http://127.0.0.1:8000`，你应该会看到以下 JSON 响应：

     ```json
     {
       "message": "Hello, FastAPI!"
     }
     ```

3. **探索自动生成的文档**

   - **Swagger UI**：访问 `http://127.0.0.1:8000/docs`
   - **ReDoc**：访问 `http://127.0.0.1:8000/redoc`

#### **思考与提问**

- 你是否理解了 FastAPI 项目的基本结构？
- 是否遇到了任何问题或错误？

---

### **第二步：构建你的第一个 API 端点**

#### **目标**

- 学习如何定义不同的 HTTP 请求方法（GET、POST、PUT、DELETE 等）
- 理解路径操作装饰器的使用

#### **内容介绍**

在 FastAPI 中，我们使用路径操作装饰器（如 `@app.get()`、`@app.post()`）来定义 API 端点。每个装饰器对应一个 HTTP 方法。

#### **练习**

1. **添加更多的端点到 `main.py`**

   ```python
   from fastapi import FastAPI

   app = FastAPI()

   @app.get("/")
   async def read_root():
       return {"message": "Hello, FastAPI!"}

   @app.get("/items")
   async def read_items():
       return [{"item_id": "Foo"}, {"item_id": "Bar"}]

   @app.post("/items")
   async def create_item(item: dict):
       return {"item_id": "NewItem", "item": item}

   @app.get("/items/{item_id}")
   async def read_item(item_id: str):
       return {"item_id": item_id}

   @app.put("/items/{item_id}")
   async def update_item(item_id: str, item: dict):
       return {"item_id": item_id, "item": item}
   ```

2. **测试新的端点**

   - **获取所有 items**：`GET http://127.0.0.1:8000/items`
   - **创建新 item**：`POST http://127.0.0.1:8000/items`
     - 在请求体中传递 JSON 数据
   - **获取特定 item**：`GET http://127.0.0.1:8000/items/42`
   - **更新 item**：`PUT http://127.0.0.1:8000/items/42`
     - 在请求体中传递更新的 JSON 数据

3. **使用 Swagger UI 测试**

   在 Swagger UI 中，可以方便地测试这些端点，并查看请求和响应的格式。

#### **思考与提问**

- 你是否理解了如何定义不同的 HTTP 请求方法？
- 对于路径参数（如 `{item_id}`），你是否清楚它们的用法？

---

### **下一步计划**

在你完成上述练习，并确认理解了内容后，我们将继续以下步骤：

3. **路径参数和查询参数的使用**
4. **请求体和数据模型（Pydantic）**
5. **表单数据和文件上传**
6. **处理错误和响应**
7. **中间件与依赖注入**
8. **认证与授权**
9. **与数据库的集成**
10. **部署你的 FastAPI 应用**

