# WatermarkRemover-AI 安装与使用教程 (Windows)

本教程将指导你从零开始，在 Windows 系统上配置并运行 WatermarkRemover-AI 水印移除工具。

## 📋 准备工作

- 一台装有 Windows 系统的电脑
- 稳定的网络环境（部分依赖包和 AI 模型需从海外服务器下载，建议自备代理）

---

## 🚀 详细步骤

### 步骤一：下载项目源码

访问项目 GitHub 页面，点击 **Code** → **Download ZIP**，下载源码压缩包并解压到本地目录（例如桌面）。

> 🔗 项目地址：[https://github.com/D-Ogi/WatermarkRemover-AI](https://github.com/D-Ogi/WatermarkRemover-AI)

---

### 步骤二：安装 Python 3.11

> ⚠️ **重要提示**：本项目依赖的 `Pillow==9.5.0` 仅支持 Python 3.11，请勿安装其他版本。

1. 打开 Python 官方 Windows 下载页面：[https://www.python.org/downloads/windows/](https://www.python.org/downloads/windows/)
2. 在版本列表中找到 **Python 3.11.9**，点击进入详情页。
3. 在页面底部的 **Files** 表格中，点击下载 **Windows installer (64-bit)**。
4. 双击运行安装程序：
   - ✅ **务必勾选**「Add Python 3.11 to PATH」
   - 点击「Install Now」完成安装
5. 验证安装：打开命令提示符（CMD），输入以下命令：
   ```bash
   py -3.11 --version
若显示 Python 3.11.x 则安装成功。

步骤三：创建并配置虚拟环境
💡 虚拟环境可以隔离项目依赖，避免与系统其他 Python 项目冲突。

打开终端：在项目文件夹的地址栏中输入 cmd 并回车。

创建虚拟环境：

bash
py -3.11 -m venv wm_env
激活虚拟环境（根据你的终端类型选择）：

CMD 命令提示符：

bash
wm_env\Scripts\activate.bat
PowerShell：

bash
.\wm_env\Scripts\Activate.ps1
激活成功后，终端最左侧会显示 (wm_env) 标记。

安装指定版本的 Pillow：

bash
pip install Pillow==9.5.0
步骤四：安装 IOPaint 核心依赖
在激活的虚拟环境中，直接执行以下命令安装 IOPaint（AI 修复引擎）：

bash
pip install iopaint
💡 提示：torch 等大型依赖包体积较大，下载可能需要几分钟，请耐心等待。若下载缓慢，可配置国内镜像源加速：

bash
pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple
步骤五：修改依赖配置文件
在运行 setup.bat 之前，需要先修改 requirements.txt 文件，避免因 PyGObject 导致 Windows 安装失败。

在项目根目录下找到 requirements.txt 文件。

使用记事本或其他文本编辑器打开。

定位到 第 26 行（内容为 PyGObject==3.42.0），在该行最前面加上 # 将其注释掉，保存文件。

diff
# 修改前
PyGObject==3.42.0

# 修改后
# PyGObject==3.42.0
步骤六：运行安装脚本
在项目文件夹中，找到 setup.bat 文件。

右键点击，选择 「以管理员身份运行」。

脚本将自动安装其余依赖并下载所需 AI 模型。

🌐 网络建议：此步骤需要从 Hugging Face 等海外服务器下载模型文件（约 2-3GB），建议全程开启代理，否则可能因网络问题导致下载失败。

步骤七：启动工具
安装完成后，双击运行项目文件夹中的 run.bat。

脚本会启动 IOPaint 服务，并自动打开浏览器访问 http://127.0.0.1:8080。

在网页界面中上传图片、框选水印区域，即可开始 AI 去水印。

🔧 后续使用
再次使用时，无需重复安装，只需：

双击运行项目文件夹中的 run.bat。

等待服务启动后，在浏览器中访问 http://127.0.0.1:8080。

❓ 常见问题排查
问题现象	可能原因	解决方法
BackendUnavailable: Cannot import 'mesonpy'	缺少构建工具或 Python 版本不兼容	确保使用 Python 3.11，并执行 pip install meson ninja
Failed to download LaMA model	AI 模型自动下载失败	在终端中手动执行 python -m iopaint download --model lama
启动后网页无法打开	服务未正常启动	检查终端是否有报错，确认 8080 端口未被占用
📄 免责声明：本工具仅供学习研究使用。在处理图片前，请确保你拥有图片的合法版权或已获得原作者授权，请勿用于侵犯他人权益的用途。

text

这份教程把关键步骤、注意事项和常见问题都整理在了一起，方便你日后查阅或分享给其他人。如果需要调整格式或补充细节，随时告诉我。