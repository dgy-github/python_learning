## **完整安装流程（推荐）**

### **步骤1：安装Python**

```cmd
复制:: 打开Microsoft Store安装Python 3.13
start ms-windows-store://pdp/?productid=9NCVDN91XZQP
```

或从官网下载：https://www.python.org/downloads/windows/

### **步骤2：验证安装**

```
cmd复制:: 关闭当前CMD，重新打开新的CMD窗口

:: 验证Python
python --version
:: 应该显示：Python 3.13.x

:: 验证pip
pip --version
:: 应该显示：pip 24.x.x from ...
```

### **步骤3：安装项目依赖**

```
cmd复制:: 进入项目目录
cd /d D:\python1020\pythonProject

:: 创建虚拟环境
python -m venv .venv

:: 激活虚拟环境
.venv\Scripts\activate.bat

:: 安装依赖
pip install -r requirements.txt

:: 运行项目
python src\main.py
```

✅ Python已安装！
Python已经安装在你的系统中，只是没有添加到PATH环境变量。

🔧 立即解决方案
方法1：临时使用完整路径（快速测试）
复制
:: 使用完整路径运行Python
C:\Users\25376\AppData\Local\Programs\Python\Python313\python.exe --version

:: 进入项目目录
cd /d D:\python1020\pythonProject

:: 创建虚拟环境
C:\Users\25376\AppData\Local\Programs\Python\Python313\python.exe -m venv .venv

:: 激活虚拟环境
.venv\Scripts\activate.bat

:: 现在可以直接使用 python 和 pip 了
python --version
pip --version

:: 安装依赖
pip install -r requirements.txt
方法2：永久添加到PATH（推荐）
自动添加（复制整段执行）
复制
:: 添加Python到系统PATH
setx PATH "%PATH%;C:\Users\25376\AppData\Local\Programs\Python\Python313;C:\Users\25376\AppData\Local\Programs\Python\Python313\Scripts"

:: 提示：需要重启CMD才能生效
echo.
echo ========================================
echo PATH已更新！
echo 请关闭当前CMD窗口，重新打开新的CMD
echo 然后运行: python --version
echo ========================================
pause
手动添加PATH（图形界面）
打开环境变量设置

复制
:: 运行这个命令打开系统属性
rundll32 sysdm.cpl,EditEnvironmentVariables
编辑用户变量

在"用户变量"中找到 Path
点击"编辑"
点击"新建"
添加以下两行：
复制
C:\Users\25376\AppData\Local\Programs\Python\Python313
C:\Users\25376\AppData\Local\Programs\Python\Python313\Scripts
点击"确定"保存
重启CMD验证

复制
python --version
pip --version
📋 完整操作流程（推荐）
步骤1：添加PATH并重启CMD
复制
:: 1. 添加到PATH
setx PATH "%PATH%;C:\Users\25376\AppData\Local\Programs\Python\Python313;C:\Users\25376\AppData\Local\Programs\Python\Python313\Scripts"

:: 2. 关闭当前CMD窗口

:: 3. 重新打开CMD（Win + R → 输入 cmd → 回车）
步骤2：验证Python
复制
:: 在新的CMD窗口中
python --version
pip --version
步骤3：创建项目环境
复制
:: 进入项目目录
cd /d D:\python1020\pythonProject

:: 创建虚拟环境
python -m venv .venv

:: 激活虚拟环境
.venv\Scripts\activate.bat

:: 升级pip
python -m pip install --upgrade pip

:: 安装依赖
pip install -r requirements.txt

:: 运行项目
python src\main.py



# 你打开的是PowerShell，不是CMD

这个错误信息表明你打开的是 **PowerShell**，而不是 **CMD**。`setx` 命令在PowerShell中的PATH更新方式不同。

------

## 🔧 **立即解决方案**

### **方法1：在PowerShell中添加PATH（推荐）**

```powershell
# 复制整段到PowerShell执行
[Environment]::SetEnvironmentVariable(
    "Path",
    [Environment]::GetEnvironmentVariable("Path", "User") + ";C:\Users\25376\AppData\Local\Programs\Python\Python313;C:\Users\25376\AppData\Local\Programs\Python\Python313\Scripts",
    "User"
)

# 刷新当前会话的环境变量
$env:Path = [System.Environment]::GetEnvironmentVariable("Path","User") + ";" + [System.Environment]::GetEnvironmentVariable("Path","Machine")

# 验证
python --version
```

# 确保在项目根目录

cd D:\python1020\pythonProject

# 创建目录结构
New-Item -ItemType Directory -Force -Path "src"
New-Item -ItemType Directory -Force -Path "tests"
New-Item -ItemType Directory -Force -Path "docs"
New-Item -ItemType Directory -Force -Path "data"
New-Item -ItemType Directory -Force -Path "config"

# 创建主要文件
New-Item -ItemType File -Force -Path "src\main.py"
New-Item -ItemType File -Force -Path "src\__init__.py"
New-Item -ItemType File -Force -Path "tests\__init__.py"
New-Item -ItemType File -Force -Path "tests\test_main.py"
New-Item -ItemType File -Force -Path "README.md"
New-Item -ItemType File -Force -Path ".gitignore"
New-Item -ItemType File -Force -Path "requirements.txt"

Write-Host "✅ 项目结构创建完成！" -ForegroundColor Green

