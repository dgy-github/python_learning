## UV 核心命令详解

### 1. UV 安装和添加依赖

#### 1.1 添加单个依赖

```
powershell复制# 添加生产依赖（会自动更新 pyproject.toml 和 uv.lock）
uv add fastapi

# 添加指定版本
uv add "fastapi>=0.115.0"
uv add "fastapi==0.115.12"

# 添加开发依赖
uv add --dev pytest
uv add --dev pytest-cov ruff mypy

# 添加可选依赖组
uv add --optional docs sphinx sphinx-rtd-theme
```

#### 1.2 批量添加依赖

```
powershell复制# 从 requirements.txt 安装
uv pip install -r requirements.txt

# 添加多个包
uv add fastapi uvicorn sqlmodel alembic
```

#### 1.3 移除依赖

```
powershell复制# 移除包
uv remove package-name

# 移除多个包
uv remove package1 package2 package3
```

#### 1.4 更新依赖

```
powershell复制# 更新所有依赖到最新兼容版本
uv sync --upgrade

# 更新特定包到最新版本
uv add package-name@latest

# 锁定依赖（生成 uv.lock）
uv lock

# 同步依赖（根据 uv.lock 安装）
uv sync
```

------

### 2. 生成和管理 pyproject.toml

#### 2.1 自动生成 pyproject.toml

```
powershell复制# 方式 1: 使用 uv init（推荐）
uv init

# 这会创建:
# - pyproject.toml（基础配置）
# - .python-version（Python 版本锁定）
# - README.md

# 方式 2: 从现有 requirements.txt 生成
uv pip compile requirements.txt -o pyproject.toml
```

#### 2.2 手动创建完整的 pyproject.toml

如果 `uv init` 生成的配置不够完整，可以手动创建：

```
powershell复制# 创建文件
New-Item -ItemType File -Path "pyproject.toml"
```

然后编辑内容（完整版）：

```
toml复制[project]
name = "fabric-recognition"
version = "0.1.0"
description = "Fabric recognition and management system"
readme = "README.md"
requires-python = ">=3.12"
authors = [
    {name = "Your Name", email = "your.email@example.com"}
]
license = {text = "MIT"}
keywords = ["fastapi", "fabric", "recognition"]
classifiers = [
    "Development Status :: 3 - Alpha",
    "Intended Audience :: Developers",
    "Programming Language :: Python :: 3.12",
]

dependencies = [
    "alembic>=1.16.1",
    "boto3>=1.40.55",
    "celery>=5.5.3",
    "chardet>=5.2.0",
    "fastapi[standard]>=0.115.12",
    "grpcio>=1.67.0,<=1.67.1",
    "grpcio-tools>=1.67.0,<=1.67.1",
    "openpyxl>=3.1.5",
    "pandas>=2.3.0",
    "psycopg[binary]>=3.2.9",
    "pydantic>=2.11.4",
    "pydantic-settings>=2.9.1",
    "pypdfium2==4.30.0",
    "pytest>=8.4.2",
    "pytest-cov>=7.0.0",
    "python-docx>=1.1.2",
    "python-dotenv>=1.1.1",
    "redis>=6.2.0",
    "requests>=2.32.5",
    "snowflake-id>=1.0.2",
    "sqlmodel>=0.0.24",
    "uvicorn[standard]>=0.38.0",
    "xlrd>=2.0.2",
]

[project.optional-dependencies]
dev = [
    "pytest>=8.4.2",
    "pytest-cov>=7.0.0",
    "pytest-asyncio>=0.24.0",
    "httpx>=0.27.0",
    "ruff>=0.7.0",
    "mypy>=1.13.0",
]
docs = [
    "sphinx>=7.0.0",
    "sphinx-rtd-theme>=2.0.0",
]

[project.scripts]
# 定义命令行工具
fabric-server = "app.main:main"

[project.urls]
Homepage = "https://github.com/yourusername/fabric-recognition"
Documentation = "https://fabric-recognition.readthedocs.io"
Repository = "https://github.com/yourusername/fabric-recognition"
Issues = "https://github.com/yourusername/fabric-recognition/issues"

# UV 配置
[[tool.uv.index]]
url = "https://pypi.tuna.tsinghua.edu.cn/simple"
default = true

[tool.uv]
dev-dependencies = [
    "pytest>=8.4.2",
    "pytest-cov>=7.0.0",
    "ruff>=0.7.0",
]

# 构建系统
[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"

# Ruff 配置（代码检查和格式化）
[tool.ruff]
line-length = 120
target-version = "py312"
exclude = [
    ".git",
    ".venv",
    "__pycache__",
    "build",
    "dist",
]

[tool.ruff.lint]
select = [
    "E",   # pycodestyle errors
    "W",   # pycodestyle warnings
    "F",   # pyflakes
    "I",   # isort
    "B",   # flake8-bugbear
    "C4",  # flake8-comprehensions
    "UP",  # pyupgrade
]
ignore = [
    "E501",  # line too long (handled by formatter)
    "B008",  # do not perform function calls in argument defaults
]

[tool.ruff.lint.per-file-ignores]
"__init__.py" = ["F401"]  # unused imports in __init__.py

[tool.ruff.format]
quote-style = "double"
indent-style = "space"

# Pytest 配置
[tool.pytest.ini_options]
testpaths = ["tests"]
python_files = "test_*.py"
python_classes = "Test*"
python_functions = "test_*"
addopts = [
    "-v",
    "--strict-markers",
    "--tb=short",
    "--cov=app",
    "--cov-report=term-missing",
    "--cov-report=html",
]
markers = [
    "slow: marks tests as slow (deselect with '-m \"not slow\"')",
    "integration: marks tests as integration tests",
    "unit: marks tests as unit tests",
]

# Coverage 配置
[tool.coverage.run]
source = ["app"]
omit = [
    "*/tests/*",
    "*/__pycache__/*",
    "*/venv/*",
    "*/.venv/*",
]

[tool.coverage.report]
exclude_lines = [
    "pragma: no cover",
    "def __repr__",
    "raise AssertionError",
    "raise NotImplementedError",
    "if __name__ == .__main__.:",
    "if TYPE_CHECKING:",
]

# MyPy 配置（类型检查）
[tool.mypy]
python_version = "3.12"
warn_return_any = true
warn_unused_configs = true
disallow_untyped_defs = false
ignore_missing_imports = true
```

#### 2.3 从现有项目迁移到 UV

```
powershell复制# 如果你有 requirements.txt
uv pip compile requirements.txt -o pyproject.toml

# 如果你有 setup.py
# 手动将依赖迁移到 pyproject.toml

# 然后锁定依赖
uv lock

# 安装依赖
uv sync
```

------

### 3. 启动服务器（更换端口）

#### 3.1 使用脚本启动（支持自定义端口）

```
powershell复制# 默认端口 8000
.\scripts\start-dev.ps1

# 自定义端口 9000
.\scripts\start-dev.ps1 -Port 9000

# 自定义主机和端口
.\scripts\start-dev.ps1 -Host 127.0.0.1 -Port 9000

# 禁用热重载
.\scripts\start-dev.ps1 -NoReload

# 生产模式（多 Worker）
.\scripts\start-dev.ps1 -NoReload -Workers 4

# 组合使用
.\scripts\start-dev.ps1 -Port 9000 -NoReload -Workers 2
```

#### 3.2 直接使用 uvicorn 命令

```
powershell复制# 激活虚拟环境
.\.venv\Scripts\Activate.ps1

# 设置 PYTHONPATH
$env:PYTHONPATH = $PWD

# 启动服务器（默认端口 8000）
uvicorn app.main:app --reload

# 自定义端口
uvicorn app.main:app --host 0.0.0.0 --port 9000 --reload

# 生产模式（多 Worker）
uvicorn app.main:app --host 0.0.0.0 --port 8000 --workers 4

# 指定日志级别
uvicorn app.main:app --log-level debug --reload

# 使用 SSL
uvicorn app.main:app --ssl-keyfile=./key.pem --ssl-certfile=./cert.pem
```

#### 3.3 在 PyCharm 运行配置中更换端口

1. 打开运行配置：`Run` → `Edit Configurations`
2. 选择 **`FastAPI Dev Server`**
3. 修改 **Parameters**:

```
makefile复制# 原来
app.main:app --host 0.0.0.0 --port 8000 --reload

# 改为
app.main:app --host 0.0.0.0 --port 9000 --reload
```

1. 点击 **`OK`** 保存

#### 3.4 使用环境变量控制端口

修改 `app/main.py`，支持环境变量：

```
python复制import os
from fastapi import FastAPI

app = FastAPI()

if __name__ == "__main__":
    import uvicorn
    
    # 从环境变量读取配置
    host = os.getenv("HOST", "0.0.0.0")
    port = int(os.getenv("PORT", "8000"))
    reload = os.getenv("RELOAD", "true").lower() == "true"
    
    uvicorn.run(
        "app.main:app",
        host=host,
        port=port,
        reload=reload
    )
```

然后启动：

```
powershell复制# 设置环境变量
$env:PORT = "9000"

# 运行
python app/main.py
```

------

### 4. UV 常用命令速查表

#### 4.1 项目初始化

```
powershell复制# 初始化新项目
uv init

# 初始化并指定 Python 版本
uv init --python 3.12

# 初始化并创建虚拟环境
uv init && uv venv && uv sync
```

#### 4.2 虚拟环境管理

```
powershell复制# 创建虚拟环境
uv venv

# 指定 Python 版本
uv venv --python 3.12

# 指定虚拟环境路径
uv venv .venv-custom

# 删除虚拟环境
Remove-Item -Recurse -Force .venv
```

#### 4.3 依赖管理

```
powershell复制# 添加依赖
uv add package-name
uv add "package-name>=1.0.0"
uv add package1 package2 package3

# 添加开发依赖
uv add --dev pytest ruff mypy

# 移除依赖
uv remove package-name

# 更新依赖
uv sync --upgrade
uv add package-name@latest

# 锁定依赖
uv lock

# 同步依赖（安装）
uv sync

# 只安装生产依赖
uv sync --no-dev

# 导出依赖
uv export > requirements.txt
uv export --no-hashes > requirements.txt
```

#### 4.4 运行命令

```
powershell复制# 在虚拟环境中运行命令
uv run python script.py
uv run pytest
uv run ruff check .

# 运行 Python 模块
uv run -m pytest
uv run -m uvicorn app.main:app --reload
```

#### 4.5 包管理

```
powershell复制# 列出已安装的包
uv pip list

# 显示包信息
uv pip show package-name

# 搜索包
uv pip search package-name

# 冻结依赖
uv pip freeze > requirements.txt
```

------

### 5. 完整的项目启动流程

#### 5.1 首次启动（完整流程）

```
powershell复制# 1. 克隆或创建项目
cd D:\pythonProject
mkdir fabric-recognition
cd fabric-recognition

# 2. 初始化 UV 项目
uv init

# 3. 创建虚拟环境
uv venv

# 4. 激活虚拟环境
.\.venv\Scripts\Activate.ps1

# 5. 添加依赖（会自动更新 pyproject.toml）
uv add fastapi "uvicorn[standard]" sqlmodel alembic

# 6. 安装所有依赖
uv sync

# 7. 创建 .env 文件
Copy-Item .env.example .env

# 8. 运行数据库迁移
alembic upgrade head

# 9. 启动服务器
.\scripts\start-dev.ps1
# 或
uvicorn app.main:app --reload
```

#### 5.2 日常开发流程

```
powershell复制# 1. 激活虚拟环境
.\.venv\Scripts\Activate.ps1

# 2. 拉取最新代码
git pull

# 3. 同步依赖
uv sync

# 4. 运行迁移
alembic upgrade head

# 5. 启动服务器
.\scripts\start-dev.ps1 -Port 8000
```

#### 5.3 添加新功能流程

```
powershell复制# 1. 创建新分支
git checkout -b feature/new-feature

# 2. 添加新依赖（如果需要）
uv add new-package

# 3. 开发代码...

# 4. 运行测试
.\scripts\run-tests.ps1

# 5. 格式化代码
uv run ruff format .

# 6. 检查代码
uv run ruff check . --fix

# 7. 提交代码
git add .
git commit -m "feat: add new feature"
git push origin feature/new-feature
```

------

### 6. 端口管理最佳实践

#### 6.1 创建端口配置文件

创建 `.env.ports`:

```
bash复制# .env.ports
API_PORT=8000
GRPC_PORT=8001
CELERY_FLOWER_PORT=5555
REDIS_PORT=6379
POSTGRES_PORT=5432
```

#### 6.2 在代码中读取端口配置

修改 `app/configs/deploy/__init__.py`:

```
python复制from pydantic import Field
from pydantic_settings import BaseSettings


class DeploymentConfig(BaseSettings):
    DEBUG: bool = Field(
        description="是否开启调试模式",
        default=False,
    )
    
    # ✅ 新增：API 端口配置
    API_PORT: int = Field(
        description="API 服务端口",
        default=8000,
    )
    
    API_HOST: str = Field(
        description="API 服务主机",
        default="0.0.0.0",
    )
    
    GRPC_PORT: str = Field(
        description="GRPC的端口",
        default="8001",
    )
    
    GRPC_MAX_WORKERS: int = Field(
        description="GRPC的workers数量",
        default=10,
    )
    
    ACCOUNT_GRPC_CLIENT_SERVER: str = Field(
        description="account-grpc服务地址",
        default="localhost:8001",
    )
```

然后在 `app/main.py` 中使用：

```
python复制from app.configs import app_config

if __name__ == "__main__":
    import uvicorn
    
    uvicorn.run(
        "app.main:app",
        host=app_config.API_HOST,
        port=app_config.API_PORT,
        reload=app_config.DEBUG
    )
```

#### 6.3 更新启动脚本支持配置文件

修改 `scripts/start-dev.ps1`，添加从 `.env` 读取端口的功能：

```
powershell复制# 在脚本开头添加
if (Test-Path "$PROJECT_ROOT\.env") {
    Get-Content "$PROJECT_ROOT\.env" | ForEach-Object {
        if ($_ -match '^API_PORT=(\d+)$') {
            $Port = [int]$Matches[1]
            Write-Host "  📌 Using port from .env: $Port" -ForegroundColor Cyan
        }
    }
}
```

------

### 7. 快速参考卡片

#### UV 命令速查

```
powershell复制# 项目管理
uv init                          # 初始化项目
uv venv                          # 创建虚拟环境
uv sync                          # 安装依赖
uv sync --upgrade                # 更新依赖

# 依赖管理
uv add package-name              # 添加依赖
uv add --dev package-name        # 添加开发依赖
uv remove package-name           # 移除依赖
uv lock                          # 锁定依赖

# 运行命令
uv run python script.py          # 运行 Python 脚本
uv run pytest                    # 运行测试
uv run uvicorn app.main:app --reload  # 启动服务器

# 导出依赖
uv export > requirements.txt     # 导出依赖
uv pip freeze > requirements.txt # 冻结依赖
```

#### 启动服务器速查

```
powershell复制# 使用脚本
.\scripts\start-dev.ps1                    # 默认 8000
.\scripts\start-dev.ps1 -Port 9000         # 自定义端口
.\scripts\start-dev.ps1 -NoReload          # 禁用热重载

# 直接使用 uvicorn
uvicorn app.main:app --reload              # 默认 8000
uvicorn app.main:app --port 9000 --reload # 自定义端口
uvicorn app.main:app --workers 4           # 多 Worker
```

------

## 总结

现在您有了完整的 UV 使用指南，包括：

✅ **依赖管理**：添加、移除、更新依赖
 ✅ **pyproject.toml 生成**：自动和手动创建
 ✅ **端口配置**：多种方式更换服务器端口
 ✅ **完整工作流**：从初始化到部署的全流程
 ✅ **最佳实践**：端口管理、环境配置等