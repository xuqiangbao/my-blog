---
title: watermark 安装
published: 2026-04-11
description: 移除水印软件安装
tags: [技术, AI]
category: AI
draft: false
---
# WatermarkRemover-AI 安装与使用教程

本教程将指导你如何在 Windows 系统上安装和运行基于 AI 的去水印工具 **WatermarkRemover-AI**。

---

## 📋 前置要求

- Windows 操作系统
- 网络连接（建议挂代理以加速依赖下载）

---

## 步骤一：下载项目代码

访问 GitHub 仓库下载源代码：

🔗 [https://github.com/D-Ogi/WatermarkRemover-AI](https://github.com/D-Ogi/WatermarkRemover-AI)

点击页面右上角的 **Code** → **Download ZIP**，解压到本地文件夹。

---

## 步骤二：安装 Python 3.11

> ⚠️ **重要提示**：本项目依赖 `Pillow==9.5.0`，该版本仅支持 Python 3.11，请务必安装正确版本！

1. 访问 Python 官方下载页面：
   🔗 [https://www.python.org/downloads/windows/](https://www.python.org/downloads/windows/)

2. 下载并安装 **Python 3.11.x** 版本

3. **安装时务必勾选**：`Add Python to PATH`

   ![安装选项示意图](https://docs.python.org/3/_images/win_installer.png)

4. 验证安装是否成功：
   ```cmd
   py -3.11 --version
   ```

   应显示类似输出：
   ```
   Python 3.11.x
   ```

---

## 步骤三：建立虚拟环境

打开 CMD 终端，依次执行以下命令：

### 1. 创建虚拟环境
```cmd
py -3.11 -m venv wm_env
```

### 2. 激活虚拟环境（CMD 方式）
```cmd
wm_env\Scripts\activate.bat
```

### 3. 激活虚拟环境（PowerShell 方式，如 CMD 方式无效）
```powershell
.\wm_env\Scripts\Activate.ps1
```

> 💡 **提示**：激活成功后，命令行前缀会显示 `(wm_env)`

### 4. 安装指定版本的 Pillow
```cmd
pip install Pillow==9.5.0
```

---

## 步骤四：安装 IOPaint

在已激活的虚拟环境中执行：

```cmd
pip install iopaint
```

---

## 步骤五：修改配置文件

> ⚠️ **重要步骤**：在运行安装前需要修改 `requirements.txt`

1. 打开项目文件夹中的 `requirements.txt` 文件
2. 找到第 26 行（`PyGObject`）
3. **注释掉该行**，在前面添加 `#`：

   ```diff
   - PyGObject==3.44.1
   + # PyGObject==3.44.1
   ```

4. 保存文件

---

## 步骤六：运行安装脚本

1. 进入 WatermarkRemover-AI 项目文件夹
2. 双击运行 `setup.bat`

> 🌐 **建议**：挂代理运行，否则部分依赖包可能因网络问题下载失败

等待安装完成...

---

## 步骤七：启动程序

安装完成后，双击运行 `run.bat` 即可启动去水印工具。

---

## 🎯 快速命令汇总

```cmd
# 验证 Python 版本
py -3.11 --version

# 创建虚拟环境
py -3.11 -m venv wm_env

# 激活虚拟环境 (CMD)
wm_env\Scripts\activate.bat

# 激活虚拟环境 (PowerShell)
.\wm_env\Scripts\Activate.ps1

# 安装依赖
pip install Pillow==9.5.0
pip install iopaint

# 运行程序
setup.bat  # 首次安装
run.bat    # 日常使用
```

---

## ❗ 常见问题

| 问题 | 解决方案 |
|------|----------|
| Python 版本错误 | 确认安装的是 3.11 版本，并使用 `py -3.11` 命令 |
| Pillow 安装失败 | 检查 Python 版本是否为 3.11 |
| 依赖下载慢/失败 | 挂代理或更换 pip 镜像源 |
| PowerShell 执行策略限制 | 以管理员身份运行 `Set-ExecutionPolicy RemoteSigned` |
| PyGObject 报错 | 确认已注释 requirements.txt 第 26 行 |

---

## 📁 项目结构参考

```
WatermarkRemover-AI/
├── wm_env/              # 虚拟环境文件夹（自动生成）
├── setup.bat            # 安装脚本
├── run.bat              # 启动脚本
├── requirements.txt     # 依赖配置文件（需修改）
└── ...                  # 其他项目文件
```

---

## 🔗 相关链接

- 项目仓库：[https://github.com/D-Ogi/WatermarkRemover-AI](https://github.com/D-Ogi/WatermarkRemover-AI)
- Python 下载：[https://www.python.org/downloads/windows/](https://www.python.org/downloads/windows/)

---

*教程生成时间：2026-04-11*
