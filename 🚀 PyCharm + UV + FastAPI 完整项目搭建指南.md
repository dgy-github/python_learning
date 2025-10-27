# 🚀 PyCharm + UV + FastAPI 完整项目搭建指南

## 📋 目录

1. [环境准备](#1-环境准备)
2. [创建项目](#2-创建项目)
3. [配置 UV 和虚拟环境](#3-配置-uv-和虚拟环境)
4. [项目结构说明](#4-项目结构说明)
5. [配置 PyCharm](#5-配置-pycharm)
6. [数据库迁移](#6-数据库迁移)
7. [开发工作流](#7-开发工作流)
8. [调试和运行](#8-调试和运行)
9. [最佳实践](#9-最佳实践)

------

## 1. 环境准备

### 1.1 安装必要软件

#### ✅ 检查清单

-  Python 3.12+ 已安装
-  PyCharm Professional/Community 已安装
-  UV 包管理器已安装
-  Git 已安装
-  PostgreSQL 客户端已安装（可选）

#### 安装 UV

```powershell
# PowerShell (管理员模式)
powershell -c "irm https://astral.sh/uv/install.ps1 | iex"

# 验证安装
uv --version
# 输出: uv 0.5.x
```

#### 验证 Python

```powershell
python --version
# 输出: Python 3.12.x
```

------

## 2. 创建项目

### 2.1 使用 PyCharm 创建新项目

#### 步骤 1: 启动 PyCharm

1. 打开 PyCharm
2. 点击 **`New Project`** 或 `File` → `New` → `Project`

#### 步骤 2: 配置项目设置

```
sql复制┌─────────────────────────────────────────────────────┐
│ New Project                                         │
├─────────────────────────────────────────────────────┤
│ Location: D:\pythonProject\fabric-recognition      │
│                                                     │
│ ○ New environment using                            │
│   ● Virtualenv                                     │
│     Location: D:\...\fabric-recognition\.venv     │
│     Base interpreter: Python 3.12                  │
│     ☐ Inherit global site-packages                │
│     ☐ Make available to all projects              │
│                                                     │
│ ○ Previously configured interpreter                │
│                                                     │
│ ☐ Create a main.py welcome script                 │
│ ☑ Create a Git repository                         │
│                                                     │
│                              [Cancel]  [Create]    │
└─────────────────────────────────────────────────────┘
```

**配置说明**:

- **Location**: 选择项目路径
- **New environment using**: 选择 `Virtualenv`
- **Base interpreter**: 选择 Python 3.12+
- **取消勾选**: `Create a main.py welcome script`
- **勾选**: `Create a Git repository`

#### 步骤 3: 点击 Create

------

## 3. 配置 UV 和虚拟环境

### 3.1 删除默认虚拟环境

```powershell
复制# 在项目根目录打开 Terminal (Alt + F12)
Remove-Item -Recurse -Force .venv
```

### 3.2 使用 UV 初始化项目

```powershell
复制# 初始化 UV 项目
uv init

# 输出:
# Initialized project `fabric-recognition`
```

### 3.3 复制您的 pyproject.toml（非必要 uv init 时候回自动生成）

创建 `pyproject.toml` 文件（基于您的配置）:

```
toml复制[project]
name = "fabric-recognition"
version = "0.1.0"
description = "Fabric recognition and management system"
readme = "README.md"
requires-python = ">=3.12"
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

[[tool.uv.index]]
url = "https://pypi.tuna.tsinghua.edu.cn/simple"
default = true

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"

[tool.ruff]
line-length = 120
target-version = "py312"

[tool.ruff.lint]
select = ["E", "F", "I"]
ignore = []

[tool.pytest.ini_options]
testpaths = ["tests"]
python_files = "test_*.py"
python_classes = "Test*"
python_functions = "test_*"
```

### 3.4 创建虚拟环境

```
powershell复制# 使用 UV 创建虚拟环境
uv venv

# 输出:
# Using Python 3.12.x interpreter at: C:\Python312\python.exe
# Creating virtualenv at: .venv
```

### 3.5 激活虚拟环境

```
powershell复制# PowerShell
.\.venv\Scripts\Activate.ps1

# 成功后提示符会显示:
# (.venv) PS D:\pythonProject\fabric-recognition>
```

### 3.6 安装依赖

```
powershell复制# 安装所有依赖
uv sync

# 输出:
# Resolved 50+ packages in 2.5s
# Installed 50+ packages in 15s
如果有新增的依赖
uv add "--依赖"
```

------

## 4. 项目结构说明

### 4.1 您的项目结构

```
csharp复制fabric-recognition/
├── .venv/                          # 虚拟环境
├── .idea/                          # PyCharm 配置
├── app/                            # 应用主目录
│   ├── __init__.py
│   ├── main.py                     # FastAPI 应用入口
│   ├── deps.py                     # 依赖注入
│   ├── alembic/                    # 数据库迁移
│   │   ├── env.py
│   │   └── versions/
│   ├── configs/                    # 配置管理
│   │   ├── __init__.py
│   │   ├── app_config.py
│   │   ├── database/
│   │   ├── deploy/
│   │   ├── feature/
│   │   └── middleware/
│   ├── constants/                  # 常量定义
│   │   ├── __init__.py
│   │   └── cache_key_template.py
│   ├── core/                       # 核心功能
│   │   ├── __init__.py
│   │   ├── database.py
│   │   ├── exception.py
│   │   └── extractor/              # 文件提取器
│   ├── extensions/                 # 扩展功能
│   │   ├── __init__.py
│   │   ├── ext_logging.py
│   │   ├── ext_redis.py
│   │   └── ext_storage.py
│   ├── grpc_services/              # gRPC 服务
│   │   ├── __init__.py
│   │   └── grpc_server.py
│   ├── lib/                        # 工具库
│   │   ├── __init__.py
│   │   ├── snowflake.py
│   │   └── ssrf_proxy.py
│   ├── models/                     # 数据模型
│   │   ├── __init__.py
│   │   ├── base.py
│   │   ├── account.py
│   │   ├── datasets.py
│   │   └── enums.py
│   ├── routers/                    # API 路由
│   │   ├── __init__.py
│   │   ├── fabric_attributes/
│   │   │   ├── __init__.py
│   │   │   └── fabric_attributes.py
│   │   └── upload_file/
│   │       ├── __init__.py
│   │       └── files.py
│   ├── schedules/                  # 定时任务
│   │   └── __init__.py
│   ├── services/                   # 业务逻辑
│   │   ├── __init__.py
│   │   ├── fabric_attribute_service.py
│   │   └── upload_file_service.py
│   └── tasks/                      # Celery 任务
│       ├── __init__.py
│       └── analyze_project.py
├── docker/                         # Docker 配置
│   ├── Dockerfile
│   └── entrypoint.sh
├── grpc/                           # gRPC 定义
│   └── account/
│       ├── __init__.py
│       ├── account_pb2.py
│       ├── account_pb2.pyi
│       └── account_pb2_grpc.py
├── tests/                          # 测试目录
│   ├── __init__.py
│   ├── test_install.py
│   └── fixed_chunk_test.py
├── scripts/                        # 工具脚本（新增）
│   ├── start-dev.ps1
│   ├── run-tests.ps1
│   └── migrate.ps1
├── .env                            # 环境变量
├── .env.example                    # 环境变量示例
├── .gitignore                      # Git 忽略文件
├── .python-version                 # Python 版本
├── alembic.ini                     # Alembic 配置
├── Dockerfile                      # Docker 配置
├── pyproject.toml                  # 项目配置
├── uv.lock                         # 依赖锁定
└── README.md                       # 项目说明
```

### 4.2 关键文件说明

| 文件/目录              | 说明                                 |
| ---------------------- | ------------------------------------ |
| `app/main.py`          | FastAPI 应用入口，定义路由和中间件   |
| `app/deps.py`          | 依赖注入，包含数据库会话、用户认证等 |
| `app/configs/`         | 配置管理，使用 Pydantic Settings     |
| `app/models/`          | SQLModel 数据模型                    |
| `app/routers/`         | API 路由定义                         |
| `app/services/`        | 业务逻辑层                           |
| `app/core/database.py` | 数据库连接配置                       |
| `app/alembic/`         | 数据库迁移脚本                       |
| `.env`                 | 环境变量配置                         |

------

## 5. 配置 PyCharm

### 5.1 配置 Python 解释器

#### 步骤 1: 打开设置

- **快捷键**: `Ctrl + Alt + S` (Windows/Linux) 或 `Cmd + ,` (Mac)

#### 步骤 2: 配置解释器

```
markdown复制Settings
├── Project: fabric-recognition
    └── Python Interpreter
```

1. 点击右上角的 **齿轮图标** ⚙️
2. 选择 **`Add Interpreter`** → **`Add Local Interpreter`**
3. 选择 **`Existing`**
4. 浏览到: `D:\pythonProject\fabric-recognition\.venv\Scripts\python.exe`
5. 点击 **`OK`**

#### 验证配置

在 PyCharm 底部状态栏应该显示:

```
scss
复制
Python 3.12 (.venv)
```

### 5.2 配置项目结构

#### 步骤 1: 打开项目结构设置

```
File` → `Settings` → `Project: fabric-recognition` → `Project Structure
```

#### 步骤 2: 标记目录

```
markdown复制✅ Sources Root:
   - app/                    # 右键 → Mark as → Sources Root

❌ Excluded:
   - .venv/                  # 右键 → Mark as → Excluded
   - __pycache__/
   - .pytest_cache/
   - .ruff_cache/
   - docker/

📁 Tests:
   - tests/                  # 右键 → Mark as → Test Sources Root
```

### 5.3 配置运行/调试

#### 配置 1: FastAPI 开发服务器

1. 点击右上角 **`Add Configuration`**
2. 点击 **`+`** → 选择 **`Python`**

```
yaml复制Name: FastAPI Dev Server
Module name: uvicorn
Parameters: app.main:app --host 0.0.0.0 --port 8000 --reload
Working directory: D:\pythonProject\fabric-recognition
Environment variables: 
  PYTHONPATH=D:\pythonProject\fabric-recognition
Python interpreter: Python 3.12 (.venv)
```

#### 配置 2: Celery Worker

```
yaml复制Name: Celery Worker
Module name: celery
Parameters: -A app.tasks.celery_app worker --loglevel=info
Working directory: D:\pythonProject\fabric-recognition
Environment variables:
  PYTHONPATH=D:\pythonProject\fabric-recognition
Python interpreter: Python 3.12 (.venv)
```

#### 配置 3: gRPC Server

```
yaml复制Name: gRPC Server
Script path: D:\pythonProject\fabric-recognition\app\grpc_services\grpc_server.py
Working directory: D:\pythonProject\fabric-recognition
Environment variables:
  PYTHONPATH=D:\pythonProject\fabric-recognition
Python interpreter: Python 3.12 (.venv)
```

### 5.4 配置环境变量

#### 创建 .env.example

```
bash复制# .env.example

# 数据库连接配置
DB_USER=pgmaster
DB_PWD=your_password_here
DB_HOST=localhost
DB_PORT=5432
DB_DATABASE=fabric_database
ECHO_SQL=false

# 云存储
S3_USE_AWS_MANAGED_IAM=false
S3_ENDPOINT=http://localhost:9000
S3_BUCKET_NAME=fabric-bucket
S3_ACCESS_KEY=minioadmin
S3_SECRET_KEY=minioadmin
S3_REGION=

# gRPC 服务地址
GRPC_PORT=8002
ACCOUNT_GRPC_CLIENT_SERVER=127.0.0.1:8001

# Celery
CELERY_BROKER_URL=amqp://guest:guest@localhost:5672//
CELERY_RESULT_BACKEND=redis://localhost:6379/0

# Milvus
MILVUS_URI=http://localhost:19530
MILVUS_TOKEN=root:Milvus
MILVUS_DATABASE=default

# Redis
REDIS_HOST=localhost
REDIS_PORT=6379
REDIS_PASSWORD=
REDIS_DB=0

# MinerU
MINERU_HOST=http://localhost
MINERU_API_PORT=38000

# File Reader
FILE_READER_ENDPOINT=http://localhost:18000
```

#### 复制并配置 .env

```
powershell复制# 复制示例文件
Copy-Item .env.example .env

# 编辑 .env 文件，填入实际配置
```

### 5.5 配置代码风格

#### Ruff 集成

1. `File` → `Settings` → `Tools` → `External Tools`
2. 点击 **`+`** 添加新工具

**Ruff Format**:

```
yaml复制Name: Ruff Format
Program: $ProjectFileDir$\.venv\Scripts\ruff.exe
Arguments: format $FilePath$
Working directory: $ProjectFileDir$
```

**Ruff Check**:

```
yaml复制Name: Ruff Check
Program: $ProjectFileDir$\.venv\Scripts\ruff.exe
Arguments: check $FilePath$ --fix
Working directory: $ProjectFileDir$
```

------

## 6. 数据库迁移

### 6.1 配置 Alembic

您的项目已经配置了 Alembic，位于 `app/alembic/`。

#### 验证配置

```
powershell复制# 查看当前迁移版本
alembic current

# 查看迁移历史
alembic history
```

### 6.2 创建数据库

```
powershell复制# 使用 psql 或 pgAdmin 创建数据库
psql -U pgmaster -h localhost -c "CREATE DATABASE fabric_database;"
```

### 6.3 运行迁移

```
powershell复制# 升级到最新版本
alembic upgrade head

# 输出:
# INFO  [alembic.runtime.migration] Running upgrade  -> 4c5e9fa2e34c, init database tables
```

### 6.4 创建新迁移

```
powershell复制# 自动生成迁移脚本
alembic revision --autogenerate -m "add new table"

# 应用迁移
alembic upgrade head

# 回滚迁移
alembic downgrade -1
```

### 6.5 创建迁移脚本

创建 `scripts/migrate.ps1`:

```
powershell复制#Requires -Version 5.1

<#
.SYNOPSIS
    数据库迁移管理脚本
.DESCRIPTION
    管理 Alembic 数据库迁移
.PARAMETER Action
    操作类型: upgrade, downgrade, current, history, revision
.PARAMETER Message
    迁移消息（用于 revision）
.EXAMPLE
    .\migrate.ps1 -Action upgrade
.EXAMPLE
    .\migrate.ps1 -Action revision -Message "add user table"
#>

param(
    [Parameter(Mandatory=$true)]
    [ValidateSet("upgrade", "downgrade", "current", "history", "revision")]
    [string]$Action,
    
    [string]$Message = ""
)

$ErrorActionPreference = "Stop"
$PROJECT_ROOT = Split-Path -Parent $PSScriptRoot

Write-Host @"
╔════════════════════════════════════════╗
║   Database Migration Manager           ║
╚════════════════════════════════════════╝
"@ -ForegroundColor Cyan

# 激活虚拟环境
if (-not $env:VIRTUAL_ENV) {
    & "$PROJECT_ROOT\.venv\Scripts\Activate.ps1"
}

# 设置 PYTHONPATH
$env:PYTHONPATH = $PROJECT_ROOT

Write-Host "`n🔧 Action: $Action" -ForegroundColor Yellow

switch ($Action) {
    "upgrade" {
        Write-Host "⬆️  Upgrading database to latest version..." -ForegroundColor Green
        alembic upgrade head
    }
    "downgrade" {
        Write-Host "⬇️  Downgrading database by 1 version..." -ForegroundColor Yellow
        alembic downgrade -1
    }
    "current" {
        Write-Host "📍 Current database version:" -ForegroundColor Cyan
        alembic current
    }
    "history" {
        Write-Host "📜 Migration history:" -ForegroundColor Cyan
        alembic history
    }
    "revision" {
        if (-not $Message) {
            Write-Host "❌ Error: Message is required for revision" -ForegroundColor Red
            exit 1
        }
        Write-Host "📝 Creating new migration: $Message" -ForegroundColor Green
        alembic revision --autogenerate -m $Message
    }
}

Write-Host "`n✅ Done!" -ForegroundColor Green
```

使用方式:

```
powershell复制# 升级数据库
.\scripts\migrate.ps1 -Action upgrade

# 创建新迁移
.\scripts\migrate.ps1 -Action revision -Message "add new table"

# 查看当前版本
.\scripts\migrate.ps1 -Action current
```

------

## 7. 开发工作流

### 7.1 创建启动脚本

创建 `scripts/start-dev.ps1`:

```powershell
#Requires -Version 5.1

<#
.SYNOPSIS
    FastAPI 开发服务器启动脚本
.DESCRIPTION
    配置环境并启动 FastAPI 开发服务器
.PARAMETER Port
    服务器端口，默认 8000
.PARAMETER Host
    服务器主机，默认 0.0.0.0
.PARAMETER NoReload
    禁用热重载
.PARAMETER Workers
    Worker 数量（生产模式）
.EXAMPLE
    .\start-dev.ps1
.EXAMPLE
    .\start-dev.ps1 -Port 9000 -NoReload
#>

[CmdletBinding()]
param(
    [int]$Port = 8000,
    [string]$Host = "0.0.0.0",
    [switch]$NoReload,
    [int]$Workers = 1
)

$ErrorActionPreference = "Stop"
$PROJECT_ROOT = Split-Path -Parent $PSScriptRoot

Write-Host @"
╔════════════════════════════════════════╗
║   Fabric Recognition - Dev Server      ║
╚════════════════════════════════════════╝
"@ -ForegroundColor Cyan

# 检查虚拟环境
Write-Host "`n[1/5] Checking virtual environment..." -ForegroundColor Yellow
if (-not (Test-Path "$PROJECT_ROOT\.venv")) {
    Write-Host "  ❌ Virtual environment not found!" -ForegroundColor Red
    Write-Host "  💡 Run: uv venv && uv sync" -ForegroundColor Gray
    exit 1
}
Write-Host "  ✅ Virtual environment found" -ForegroundColor Green

# 激活虚拟环境
Write-Host "`n[2/5] Activating virtual environment..." -ForegroundColor Yellow
if (-not $env:VIRTUAL_ENV) {
    & "$PROJECT_ROOT\.venv\Scripts\Activate.ps1"
    Write-Host "  ✅ Activated" -ForegroundColor Green
} else {
    Write-Host "  ✅ Already activated" -ForegroundColor Green
}

# 设置 PYTHONPATH
Write-Host "`n[3/5] Setting PYTHONPATH..." -ForegroundColor Yellow
$env:PYTHONPATH = $PROJECT_ROOT
Write-Host "  ✅ PYTHONPATH set to: $PROJECT_ROOT" -ForegroundColor Green

# 检查 .env 文件
Write-Host "`n[4/5] Checking .env file..." -ForegroundColor Yellow
if (-not (Test-Path "$PROJECT_ROOT\.env")) {
    Write-Host "  ⚠️  .env file not found!" -ForegroundColor Yellow
    if (Test-Path "$PROJECT_ROOT\.env.example") {
        Write-Host "  💡 Copy .env.example to .env and configure" -ForegroundColor Gray
    }
    exit 1
} else {
    Write-Host "  ✅ .env file found" -ForegroundColor Green
}

# 启动服务
Write-Host "`n[5/5] Starting server..." -ForegroundColor Yellow

$uvicornArgs = @(
    "app.main:app",
    "--host", $Host,
    "--port", $Port
)

if (-not $NoReload) {
    $uvicornArgs += "--reload"
}

if ($Workers -gt 1) {
    $uvicornArgs += "--workers", $Workers
}

Write-Host @"

╔════════════════════════════════════════╗
║  🚀 Server starting...                 ║
║  📍 http://${Host}:${Port}            ║
║  📚 Docs: http://${Host}:${Port}/fabric/redoc ║
║  📖 OpenAPI: http://${Host}:${Port}/fabric/openapi.json ║
║  ⏹️  Press Ctrl+C to stop              ║
╚════════════════════════════════════════╝

"@ -ForegroundColor Green

uvicorn @uvicornArgs
```

### 7.2 创建测试脚本

创建 `scripts/run-tests.ps1`:

```powershell
#Requires -Version 5.1

<#
.SYNOPSIS
    运行测试脚本
.DESCRIPTION
    运行项目测试套件
.PARAMETER Coverage
    生成覆盖率报告
.PARAMETER Verbose
    详细输出
.PARAMETER Pattern
    测试文件模式
.EXAMPLE
    .\run-tests.ps1
.EXAMPLE
    .\run-tests.ps1 -Coverage -Verbose
#>

[CmdletBinding()]
param(
    [switch]$Coverage,
    [switch]$Verbose,
    [string]$Pattern = "test_*.py"
)

$ErrorActionPreference = "Stop"
$PROJECT_ROOT = Split-Path -Parent $PSScriptRoot

Write-Host @"
╔════════════════════════════════════════╗
║   Running Tests                        ║
╚════════════════════════════════════════╝
"@ -ForegroundColor Cyan

# 激活虚拟环境
if (-not $env:VIRTUAL_ENV) {
    & "$PROJECT_ROOT\.venv\Scripts\Activate.ps1"
}

# 设置 PYTHONPATH
$env:PYTHONPATH = $PROJECT_ROOT

# 构建 pytest 参数
$pytestArgs = @("tests/")

if ($Verbose) {
    $pytestArgs += "-v"
}

if ($Coverage) {
    $pytestArgs += @(
        "--cov=app",
        "--cov-report=html",
        "--cov-report=term-missing"
    )
}

if ($Pattern) {
    $pytestArgs += "-k", $Pattern
}

Write-Host "`n🧪 Running pytest..." -ForegroundColor Yellow
pytest @pytestArgs

if ($LASTEXITCODE -eq 0) {
    Write-Host "`n✅ All tests passed!" -ForegroundColor Green
    
    if ($Coverage) {
        Write-Host "`n📊 Coverage report generated: htmlcov/index.html" -ForegroundColor Cyan
    }
} else {
    Write-Host "`n❌ Some tests failed!" -ForegroundColor Red
    exit 1
}
```

### 7.3 创建 Celery 启动脚本（非必要）

创建 `scripts/start-celery.ps1`:

```
powershell复制#Requires -Version 5.1

<#
.SYNOPSIS
    Celery Worker 启动脚本
.DESCRIPTION
    启动 Celery Worker
.PARAMETER Concurrency
    并发数，默认 4
.PARAMETER LogLevel
    日志级别，默认 info
.EXAMPLE
    .\start-celery.ps1
.EXAMPLE
    .\start-celery.ps1 -Concurrency 8 -LogLevel debug
#>

[CmdletBinding()]
param(
    [int]$Concurrency = 4,
    [ValidateSet("debug", "info", "warning", "error")]
    [string]$LogLevel = "info"
)

$ErrorActionPreference = "Stop"
$PROJECT_ROOT = Split-Path -Parent $PSScriptRoot

Write-Host @"
╔════════════════════════════════════════╗
║   Celery Worker Launcher               ║
╚════════════════════════════════════════╝
"@ -ForegroundColor Cyan

# 激活虚拟环境
if (-not $env:VIRTUAL_ENV) {
    & "$PROJECT_ROOT\.venv\Scripts\Activate.ps1"
}

# 设置 PYTHONPATH
$env:PYTHONPATH = $PROJECT_ROOT

Write-Host "`n🚀 Starting Celery Worker..." -ForegroundColor Yellow
Write-Host "  Concurrency: $Concurrency" -ForegroundColor Gray
Write-Host "  Log Level: $LogLevel" -ForegroundColor Gray

celery -A app.tasks.celery_app worker `
    --loglevel=$LogLevel `
    --concurrency=$Concurrency `
    --pool=solo
```

### 7.4 创建 .gitignore（非必要）

```
gitignore复制# Python
__pycache__/
*.py[cod]
*$py.class
*.so
.Python
build/
develop-eggs/
dist/
downloads/
eggs/
.eggs/
lib/
lib64/
parts/
sdist/
var/
wheels/
*.egg-info/
.installed.cfg
*.egg

# Virtual Environment
.venv/
venv/
ENV/
env/

# UV
uv.lock

# PyCharm
.idea/
*.iml
*.iws
.idea_modules/

# VS Code
.vscode/

# Environment
.env
.env.local

# Testing
.pytest_cache/
.coverage
htmlcov/
.tox/

# Ruff
.ruff_cache/

# Database
*.db
*.sqlite
*.sqlite3

# Logs
*.log
logs/

# OS
.DS_Store
Thumbs.db

# Alembic
alembic/versions/*.pyc

# Celery
celerybeat-schedule
celerybeat.pid

# Docker
.dockerignore
```

------

## 8. 调试和运行

### 8.1 在 PyCharm 中运行

#### 方式 1: 使用运行配置

1. 点击右上角的运行配置下拉菜单
2. 选择 **`FastAPI Dev Server`**
3. 点击 **绿色运行按钮** ▶️ 或按 `Shift + F10`

#### 方式 2: 使用 Terminal

```powershell
# 在 PyCharm Terminal (Alt + F12)
.\scripts\start-dev.ps1
```

#### 方式 3: 直接运行 main.py

右键点击 `app/main.py` → `Run 'main'`

### 8.2 调试应用

#### 设置断点

1. 在代码行号左侧点击，设置红色断点 🔴
2. 例如在 `app/routers/fabric_attributes/fabric_attributes.py` 的 `create_fabric_attribute` 函数中设置断点

#### 启动调试

1. 选择运行配置 **`FastAPI Dev Server`**
2. 点击 **调试按钮** 🐛 或按 `Shift + F9`

#### 调试操作

- **F8**: Step Over (单步跳过)
- **F7**: Step Into (单步进入)
- **Shift + F8**: Step Out (单步跳出)
- **F9**: Resume Program (继续执行)
- **Ctrl + F8**: Toggle Breakpoint (切换断点)

### 8.3 测试 API

#### 使用 ReDoc

1. 启动服务后访问: http://localhost:8000/fabric/redoc
2. 查看 API 文档

#### 使用 PyCharm HTTP Client

创建 `api-tests.http`:

```
http复制### 健康检查
GET http://localhost:8000/
Authorization: Bearer your-token-here

### 上传文件
POST http://localhost:8000/n-fabric/api/files/upload
Authorization: Bearer your-token-here
Content-Type: multipart/form-data; boundary=----WebKitFormBoundary7MA4YWxkTrZu0gW

------WebKitFormBoundary7MA4YWxkTrZu0gW
Content-Disposition: form-data; name="file"; filename="test.txt"
Content-Type: text/plain

< ./test.txt
------WebKitFormBoundary7MA4YWxkTrZu0gW--

### 创建面料属性
POST http://localhost:8000/n-fabric/api/fabric-attributes/create
Authorization: Bearer your-token-here
Content-Type: application/json

{
  "product_name": "纯棉斜纹布",
  "production_code": "CT-2024-001",
  "composition": "100%棉",
  "weight": "200g/m²",
  "color": "藏青色",
  "elasticity": "no_stretch",
  "pattern": "twill",
  "remark": "适合春夏季节",
  "images": [
    {
      "upload_file_id": "7386633859489767424",
      "image_type": "main",
      "sort_order": 0,
      "description": "正面图"
    }
  ]
}

### 查询面料属性列表
GET http://localhost:8000/n-fabric/api/fabric-attributes?page=1&page_size=20
Authorization: Bearer your-token-here

### 查询面料属性详情
GET http://localhost:8000/n-fabric/api/fabric-attributes/{fabric_attribute_id}
Authorization: Bearer your-token-here
```

### 8.4 运行测试

#### 方式 1: 使用脚本

```
powershell复制# 运行所有测试
.\scripts\run-tests.ps1

# 带覆盖率
.\scripts\run-tests.ps1 -Coverage

# 详细输出
.\scripts\run-tests.ps1 -Verbose
```

#### 方式 2: 使用 PyCharm 测试运行器

1. 右键点击 `tests` 目录
2. 选择 **`Run 'pytest in tests'`**

#### 方式 3: 使用 Terminal

```
powershell复制# 运行所有测试
pytest

# 运行特定文件
pytest tests/test_install.py

# 详细输出
pytest -v

# 带覆盖率
pytest --cov=app --cov-report=html
```

------

## 9. 最佳实践

### 9.1 代码质量

#### 使用 Ruff 格式化

```
powershell复制# 格式化所有代码
uv run ruff format .

# 检查代码
uv run ruff check .

# 自动修复
uv run ruff check . --fix
```

#### 在 PyCharm 中使用

- **格式化当前文件**: `Tools` → `External Tools` → `Ruff Format`
- **或使用快捷键**: `Ctrl + Alt + L`

### 9.2 依赖管理

#### 添加新依赖

```
powershell复制# 添加生产依赖
uv add package-name

# 添加开发依赖
uv add --dev package-name

# 从 requirements.txt 导入
uv pip install -r requirements.txt
```

#### 更新依赖

```
powershell复制# 更新所有依赖
uv sync --upgrade

# 更新特定包
uv add package-name@latest
```

#### 导出依赖

```
powershell复制# 导出到 requirements.txt
uv pip freeze > requirements.txt

# 或使用 UV 的导出功能
uv export --no-hashes > requirements.txt
```

### 9.3 环境管理

#### 多环境配置

创建不同的 `.env` 文件:

```
bash复制# .env.development
DEBUG=True
LOG_LEVEL=DEBUG
DB_DATABASE=fabric_database_dev

# .env.production
DEBUG=False
LOG_LEVEL=INFO
DB_DATABASE=fabric_database_prod
```

在启动时指定:

```
powershell复制# 开发环境
$env:ENV_FILE_PATH = ".env.development"
.\scripts\start-dev.ps1

# 生产环境
$env:ENV_FILE_PATH = ".env.production"
.\scripts\start-dev.ps1 -NoReload -Workers 4
```

### 9.4 日志配置

您的项目已经配置了日志系统（`app/extensions/ext_logging.py`）。

#### 使用日志

```
python复制import logging

logger = logging.getLogger(__name__)

# 不同级别的日志
logger.debug("调试信息")
logger.info("普通信息")
logger.warning("警告信息")
logger.error("错误信息")
logger.exception("异常信息（带堆栈）")
```

### 9.5 Docker 支持

您的项目已经有 Docker 配置（`docker/Dockerfile`）。

#### 构建镜像

```
powershell复制# 构建镜像
docker build -t fabric-recognition:latest -f docker/Dockerfile .

# 运行容器
docker run -d `
  --name fabric-recognition `
  -p 8000:8000 `
  --env-file .env `
  fabric-recognition:latest
```

#### docker-compose.yml

创建 `docker-compose.yml`:

```
复制version: '3.8'

services:
  api:
    build:
      context: .
      dockerfile: docker/Dockerfile
    ports:
      - "8000:8000"
    environment:
      - DB_HOST=db
      - REDIS_HOST=redis
      - CELERY_BROKER_URL=amqp://rabbitmq:5672//
    env_file:
      - .env
    depends_on:
      - db
      - redis
      - rabbitmq
    volumes:
      - ./app:/app/app
  
  celery-worker:
    build:
      context: .
      dockerfile: docker/Dockerfile
    command: celery -A app.tasks.celery_app worker --loglevel=info
    environment:
      - DB_HOST=db
      - REDIS_HOST=redis
      - CELERY_BROKER_URL=amqp://rabbitmq:5672//
    env_file:
      - .env
    depends_on:
      - db
      - redis
      - rabbitmq
  
  db:
    image: postgres:15
    environment:
      - POSTGRES_USER=pgmaster
      - POSTGRES_PASSWORD=pgmaster
      - POSTGRES_DB=fabric_database
    volumes:
      - postgres_data:/var/lib/postgresql/data
    ports:
      - "5432:5432"
  
  redis:
    image: redis:7-alpine
    ports:
      - "6379:6379"
  
  rabbitmq:
    image: rabbitmq:3-management
    ports:
      - "5672:5672"
      - "15672:15672"

volumes:
  postgres_data:
```

使用:

```
powershell复制# 启动所有服务
docker-compose up -d

# 查看日志
docker-compose logs -f api

# 停止服务
docker-compose down
```

------

## 10. 常见问题

### 10.1 导入错误

**问题**: `ModuleNotFoundError: No module named 'app'`

**解决方案**:

1. 确保项目根目录被标记为 Sources Root
2. 检查 PYTHONPATH 配置
3. 在 Terminal 中从项目根目录运行

```
powershell复制# 设置 PYTHONPATH
$env:PYTHONPATH = "D:\pythonProject\fabric-recognition"
```

### 10.2 数据库连接错误

**问题**: `could not connect to server`

**解决方案**:

1. 检查 PostgreSQL 是否运行
2. 验证 `.env` 中的数据库配置
3. 测试连接

```
powershell复制# 测试连接
python tests/test_install.py
```

### 10.3 端口占用

**问题**: `Address already in use`

**解决方案**:

```
powershell复制# 查找占用端口的进程
netstat -ano | findstr :8000

# 结束进程
taskkill /PID <PID> /F

# 或使用不同端口
.\scripts\start-dev.ps1 -Port 8001
```

### 10.4 gRPC 连接错误

**问题**: `failed to connect to all addresses`

**解决方案**:

1. 确保 account-grpc 服务正在运行
2. 检查 `ACCOUNT_GRPC_CLIENT_SERVER` 配置
3. 验证网络连接

------

## 11. 快速参考

### 常用命令

```
powershell复制# UV 命令
uv venv                    # 创建虚拟环境
uv sync                    # 安装依赖
uv add <package>           # 添加依赖
uv remove <package>        # 移除依赖
uv run <command>           # 在虚拟环境中运行命令

# 启动服务
.\scripts\start-dev.ps1    # 开发模式
.\scripts\start-celery.ps1 # Celery Worker

# 数据库迁移
.\scripts\migrate.ps1 -Action upgrade        # 升级数据库
.\scripts\migrate.ps1 -Action revision -Message "add table"  # 创建迁移

# 测试
.\scripts\run-tests.ps1    # 运行测试
pytest                     # 直接运行
pytest -v                  # 详细输出
pytest --cov=app           # 带覆盖率

# 代码质量
uv run ruff format .       # 格式化
uv run ruff check .        # 检查
uv run ruff check . --fix  # 自动修复
```

### PyCharm 快捷键

| 功能 | Windows/Linux            | Mac               |
| ---- | ------------------------ | ----------------- |
| 运行 | `Shift + F10`            | `Ctrl + R`        |
| 调试 | `Shift + F9`             | `Ctrl + D`        |
| 停止 | `Ctrl + F2`              | `Cmd + F2`        |
| 终端 | `Alt + F12`              | `Option + F12`    |
| 设置 | `Ctrl + Alt + S`         | `Cmd + ,`         |
| 查找 | `Ctrl + Shift + F`       | `Cmd + Shift + F` |
| 重构 | `Ctrl + Alt + Shift + T` | `Ctrl + T`        |

------

## 12. 项目特定说明

### 12.1 面料属性管理

您的项目包含完整的面料属性管理功能:

- **创建面料**: `POST /n-fabric/api/fabric-attributes/create`
- **更新面料**: `PUT /n-fabric/api/fabric-attributes/{id}`
- **删除面料**: `DELETE /n-fabric/api/fabric-attributes/{id}`
- **查询列表**: `GET /n-fabric/api/fabric-attributes`
- **查询详情**: `GET /n-fabric/api/fabric-attributes/{id}`

### 12.2 文件上传

支持多种文件格式:

- 文档: `txt`, `md`, `pdf`, `docx`, `html`, `pptx`, `xml`
- 表格: `xlsx`, `csv`
- 图片: `png`, `jpg`, `jpeg`

### 12.3 gRPC 服务

项目使用 gRPC 进行服务间通信:

- **账户服务**: `app/grpc_services/grpc_server.py`
- **Proto 定义**: `grpc/account/`

### 12.4 Celery 任务

异步任务处理:

- **文档解析**: `app/tasks/document_parse_task.py`
- **文档分块**: `app/tasks/document_chunk_task.py`
- **向量嵌入**: `app/tasks/add_document_embedding_task.py`

------

## 总结

你现在已经有了一个完整的 **fabric-recognition** 项目开发环境！🎉

### ✅ 完成的内容

-  项目结构搭建
-  UV 包管理配置
-  PyCharm 开发环境配置
-  数据库迁移配置
-  FastAPI 应用开发
-  gRPC 服务集成
-  Celery 任务队列
-  测试框架集成
-  代码质量工具
-  启动脚本和工作流

### 🚀 下一步

1. **完善功能**: 继续开发面料识别功能
2. **添加测试**: 编写更多单元测试和集成测试
3. **优化性能**: 使用缓存、异步处理等
4. **部署上线**: 使用 Docker 部署到生产环境
5. **监控运维**: 添加日志监控、性能监控

祝你开发愉快！💻✨



